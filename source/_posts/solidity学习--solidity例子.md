title: solidity学习之solidity实例
category: solidity
tags: solidity
date: 2016-12-03 10:23:12
---

## 投票
下面的合约是非常复杂的，但是展示了大量的solidity的功能。它实现了一个投票合约。当然，电子投票的主要问题是如何为正确的人分为投票权利和如何防止暗箱操作，我们在这里不会解决所有的问题，但至少我们将会展示如何在自动计票和完全透明的同时委派投票。

想法是为每一张选票创建一个合约，为每个投票选项提供一个简短的名字，然后合约的创建者会作为主持人来给予每个投票参与人的地址单独投票的权利。
<!--more-->
每个地址后面的人可以将选择将票投给他们自己或者将其委托给他们信任的人。

在投票结束后，`winningProposal()`将返回票数最多的提案。


```javascript
pragma solidity ^0.4.0;

/// @title Voting with delegation.
/// @title 授权投票
contract Ballot {
    // 这里声明了一个新的复杂类型将会在后面作为变量被使用
    // 它代表单个投票人
    struct Voter {
        uint weight; // 授权累积的权重
        bool voted;  // 为 true, 则表示这个人已经投票了
        address delegate; // 委托的投票代表
        uint vote;   // 投票选择的提案索引号
    }

    // 这是单个提案的类型
    struct Proposal
    {
        bytes32 name;   // short name (up to 32 bytes)
        uint voteCount; // number of accumulated votes
    }

    address public chairperson;

    // 这里声明一个状态变量，保存每个独立地址的`Voter` 结构
    mapping(address => Voter) public voters;

    // 一个`Proposal`类型的动态数组.
    Proposal[] public proposals;

    /// 创建一个新的投票用来选出一个提案名`proposalNames`.
    function Ballot(bytes32[] proposalNames) {
        chairperson = msg.sender;
        voters[chairperson].weight = 1;

        //对提供的每一个提案名称，创建一个新的提案对象添加到数组末尾
        for (uint i = 0; i < proposalNames.length; i++) {
            //`Proposal({...})` 创建了一个临时的提案对象，
            //`proposal.push(...)`添加到了提案数组`proposals`末尾。
            proposals.push(Proposal({
                name: proposalNames[i],
                voteCount: 0
            }));
        }
    }

    // 给`voter` 投票的权利.
    // 只能由投票主持人`chairperson`调用
    function giveRightToVote(address voter) {
        if (msg.sender != chairperson || voters[voter].voted) {
            //`throw`会终止和撤销所有的状态和以太改变。
           //如果函数调用无效，这通常是一个好的选择。
           //但是需要注意，这会消耗提供的所有gas。
            throw;
        }
        voters[voter].weight = 1;
    }

    /// 委托你的投票权到一个投票代表 `to`。
    function delegate(address to) {
        // 指定引用
        Voter sender = voters[msg.sender];
        if (sender.voted)
            throw;

        //当投票代表`to`也委托给别人时，寻找到最终的投票代表
        // 通常情况下这个循环是非常危险的，因为如果他们运行太久，他们可能比在一个区块中一般情况下需要更多的gas。
        // 在这个例子中，委托将不会被执行，但是在其他场景中，这样的循环可能导致合约完全卡住（"stuck"）
        while (
            voters[to].delegate != address(0) &&
            voters[to].delegate != msg.sender
        ) {
            to = voters[to].delegate;
        }

        // 当最终投票代表等于调用者，是不被允许的。
        if (to == msg.sender) {
            throw;
        }

        //因为`sender`是一个引用，
        //这里实际修改了`voters[msg.sender].voted`
        sender.voted = true;
        sender.delegate = to;
        Voter delegate = voters[to];
        if (delegate.voted) {
            //如果委托的投票代表已经投票了，直接增加票数
            proposals[delegate.vote].voteCount += sender.weight;
        } else {
            //如果投票代表还没有投票，则增加其投票权重。
            delegate.weight += sender.weight;
        }
    }

    ///投出你的选票（包括委托给你的选票）给 `proposals[proposal].name`。
    function vote(uint proposal) {
        Voter sender = voters[msg.sender];
        if (sender.voted)
            throw;
        sender.voted = true;
        sender.vote = proposal;

        //如果`proposal`索引超出了给定的提案数组范围
        //将会自动抛出异常，并撤销所有的改变。
        proposals[proposal].voteCount += sender.weight;
    }

    ///@dev 根据当前所有的投票计算出当前的胜出提案
    function winningProposal() constant
            returns (uint winningProposal)
    {
        uint winningVoteCount = 0;
        for (uint p = 0; p < proposals.length; p++) {
            if (proposals[p].voteCount > winningVoteCount) {
                winningVoteCount = proposals[p].voteCount;
                winningProposal = p;
            }
        }
    }

    // 调用 winningProposal() 函数得到包含在提案数组中的获胜提案的索引并且然后返回获胜投案的名字
    // of the winner contained in the proposals array and then
    // returns the name of the winner
    function winnerName() constant
            returns (bytes32 winnerName)
    {
        winnerName = proposals[winningProposal()].name;
    }
}
```

### 可能的提升
目前，许多事务需要分配权利到所有的参与者，你能否想出一个更好的方法？

## 盲拍
在这部分中，我们将展示在以太坊中创建一个完整的盲拍合约是多么的简单。我们将从公开拍卖开始，每个人可以看到标价，然后将此合约扩展到盲拍，在盲拍中不能看到确切的标价一直到出价结束。

###简单的公开拍卖
下面的简单拍卖合约是每个人都可以在拍卖中给出他们的出价，出价需要包括发送钱/以太币，这是为了约束拍卖人有能力进行出价。如果最高价提高了，先前的最高价将拿回他出价的钱。此次拍卖结束后，合约必须手动的呼叫受益者拿取他的钱，合约不会自己激活自己。


```javascript
pragma solidity ^0.4.0;

contract SimpleAuction {
    // 拍卖的参数
    // 时间要么为unix绝对时间戳（自1970-01-01以来的秒数）
    // 或者是以秒为单位的出块时间
    address public beneficiary;
    uint public auctionStart;
    uint public biddingTime;

    //当前的拍卖状态
    address public highestBidder;
    uint public highestBid;

    // 允许撤销先前的出价
    mapping(address => uint) pendingReturns;

    //在结束时设置为true来拒绝任何改变
    bool ended;

    // 在发生变化时事件会被触发
    event HighestBidIncreased(address bidder, uint amount);
    event AuctionEnded(address winner, uint amount);

    //下面是一个叫做natspec的特殊注释，
    //由3个连续的斜杠标记，当询问用户确认交易事务时将显示。

    /// 创建一个简单的合约使用`_biddingTime`表示的竞拍时间，
    /// 地址`_beneficiary`.代表实际的拍卖者
    function SimpleAuction(
        uint _biddingTime,
        address _beneficiary
    ) {
        beneficiary = _beneficiary;
        auctionStart = now;
        biddingTime = _biddingTime;
    }

    ///对拍卖的竞拍保证金会随着交易事务一起发送，
    ///只有在竞拍失败的时候才会退回
    function bid() payable {
        //不需要任何参数，所有的信息已经是交易事务的一部分
        // 关键字payable说明此函数能收到以太币
        if (now > auctionStart + biddingTime) {
            //当竞拍结束时撤销此调用
            throw;
        }
        if (msg.value <= highestBid) {
            //如果出价不是最高的，发回竞拍保证金。
            throw;
        }
        if (highestBidder != 0) {
            // 通过简单的使用highestBidder.send(highestBid)
            // 会有一个安全风险，因为它可以被调用者进行阻止
            // 例如提升调用栈到1023. 
            // 让收款人自己收钱总是更安全的
            pendingReturns[highestBidder] += highestBid;
        }
        highestBidder = msg.sender;
        highestBid = msg.value;
        HighestBidIncreased(msg.sender, msg.value);
    }

    /// 撤回超价  
    function withdraw() returns (bool) {
        var amount = pendingReturns[msg.sender];
        if (amount > 0) {
            // 将这个设为0是很重要的
            // 因为它可以作为接收调用的一部分再次调用这个函数
            // 在`send`返回之前
            pendingReturns[msg.sender] = 0;

            if (!msg.sender.send(amount)) {
                // 这里不需要调用throw，只需要重新设置主人的账户
                pendingReturns[msg.sender] = amount;
                return false;
            }
        }
        return true;
    }

    ///拍卖结束后发送最高的竞价到拍卖人
    function auctionEnd() {
        // 这是一个很好的向导对于构建与其他合约交互的功能
        // (例子： 他们呢调用函数或者发送以太币)
        // 分为三个阶段：
        // 1. 判断条件
        // 2. 执行操作 (潜在改变条件)
        // 3. 与其他合约交互
        // 如果这些阶段混合了
        // 另一个合约可以回调到当前合约中并修改状态
        // 或者导致效应（以太币支付）被多次执行
        // 如果内部调用的函数包括与外部合约的交互
        // 则还必须考虑与外部合约的交互 

        // 1. 条件
        if (now <= auctionStart + biddingTime)
            throw; // auction did not yet end
        if (ended)
            throw; // this function has already been called

        // 2. 执行操作
        ended = true;
        AuctionEnded(highestBidder, highestBid);

        // 交互
        if (!beneficiary.send(highestBid))
            throw;
    }
}
```

### 盲拍
从之前的公开竞拍扩展到下面的盲拍。盲拍的优点是在竞拍时期没有时间压力，在透明的计算平台上创建盲拍，听起来可能是一个矛盾，但是可以通过加密技术解决。

在竞拍时期，竞拍者不能确切的发送她的竞拍价，而是发送竞拍价的哈希版本。由于目前考虑到实际上不能找到哈希值完全相等的（足够长的）值，所以竞拍者提交它。在竞拍结束之后，竞拍者必须公开他们的竞拍价：他们发送他们没有加密的值，然后合约检验哈希值是否与竞拍时的值是相同的。

另外一个挑战是如何使得拍卖同时是被约束和看不见的：防止竞拍者在他赢得拍卖之后不给钱的唯一方法是让他将钱一同发送。由于在以太坊中价值转移不能是不可见的，每个人都可以看到这个值。

下面的合约解决了通过接受至少与出价同样大的任何值来解决这个问题。在竞拍阶段当然是可以检查的，有些可能是无效的，有些可能是故意的（甚至提供了一个明确的标志以高价值转移为无效出价）：竞拍者可以通过放置几个或高或低的无效价格来搅乱竞争。


```javascript
pragma solidity ^0.4.0;

contract BlindAuction {
    struct Bid {
        bytes32 blindedBid;
        uint deposit;
    }

    address public beneficiary;
    uint public auctionStart;
    uint public biddingEnd;
    uint public revealEnd;
    bool public ended;

    mapping(address => Bid[]) public bids;

    address public highestBidder;
    uint public highestBid;

    // 允许撤销先前的竞拍
    mapping(address => uint) pendingReturns;

    event AuctionEnded(address winner, uint highestBid);

    /// 修饰器（Modifier）是一个简便的途径用来验证函数输入的有效性。
    /// `onlyBefore` 应用于下面的 `bid`函数
    /// 其旧的函数体替换修饰器主体中 `_`后就是其新的函数体
    modifier onlyBefore(uint _time) { if (now >= _time) throw; _; }
    modifier onlyAfter(uint _time) { if (now <= _time) throw; _; }

    function BlindAuction(
        uint _biddingTime,
        uint _revealTime,
        address _beneficiary
    ) {
        beneficiary = _beneficiary;
        auctionStart = now;
        biddingEnd = now + _biddingTime;
        revealEnd = biddingEnd + _revealTime;
    }

    /// 放置一个盲拍出价使用`_blindedBid`=sha3(value,fake,secret).
    /// 仅仅在竞拍结束正常揭拍后退还发送的以太。当随同发送的以太至少
    /// 等于 "value"指定的保证金并且 "fake"不为true的时候才是有效的竞拍
    /// 出价。设置 "fake"为true或发送不合适的金额将会掩没真正的竞拍出
    /// 价，但是仍然需要抵押保证金。同一个地址可以放置多个竞拍。
    function bid(bytes32 _blindedBid)
        payable
        onlyBefore(biddingEnd)
    {
        bids[msg.sender].push(Bid({
            blindedBid: _blindedBid,
            deposit: msg.value
        }));
    }

    /// 揭开你的盲拍竞价。你将会拿回除了最高出价外的所有竞拍保证金
    /// 以及正常的无效盲拍保证金。
    function reveal(
        uint[] _values,
        bool[] _fake,
        bytes32[] _secret
    )
        onlyAfter(biddingEnd)
        onlyBefore(revealEnd)
    {
        uint length = bids[msg.sender].length;
        if (
            _values.length != length ||
            _fake.length != length ||
            _secret.length != length
        ) {
            throw;
        }

        uint refund;
        for (uint i = 0; i < length; i++) {
            var bid = bids[msg.sender][i];
            var (value, fake, secret) =
                    (_values[i], _fake[i], _secret[i]);
            if (bid.blindedBid != keccak256(value, fake, secret)) {
                // 出价未被正常揭拍，不能取回保证金。
                continue;
            }
            refund += bid.deposit;
            if (!fake && bid.deposit >= value) {
                if (placeBid(msg.sender, value))
                    refund -= value;
            }
            // 保证发送者绝不可能重复取回保证金
            bid.blindedBid = 0;
        }
        if (!msg.sender.send(refund))
            throw;
    }

    // 这是一个内部 (internal)函数，
    // 意味着仅仅只有合约（或者从其继承的合约）可以调用
    function placeBid(address bidder, uint value) internal
            returns (bool success)
    {
        if (value <= highestBid) {
            return false;
        }
        if (highestBidder != 0) {
            // 退还前一个最高竞拍出价
            pendingReturns[highestBidder] += highestBid;
        }
        highestBid = value;
        highestBidder = bidder;
        return true;
    }

    /// 撤销不可理的出价
    function withdraw() returns (bool) {
        var amount = pendingReturns[msg.sender];
        if (amount > 0) {
            // 将这个设为0是很重要的
            // 因为它可以作为接收调用的一部分再次调用这个函数
            // 在`send`返回之前 (看上面关于
            // conditions -> effects -> interaction的备注).
            pendingReturns[msg.sender] = 0;

            if (!msg.sender.send(amount)){
                // 这里不需要调用throw
                pendingReturns[msg.sender] = amount;
                return false;
            }
        }
        return true;
    }

    /// 竞拍结束后发送最高出价到竞拍人
    function auctionEnd()
        onlyAfter(revealEnd)
    {
        if (ended)
            throw;
        AuctionEnded(highestBidder, highestBid);
        ended = true;
        // 发送合约拥有所有的钱，因为有一些保证金退回可能失败了。
        if (!beneficiary.send(this.balance))
            throw;
    }
}
```


## Safe Remote Purchase 安全的远程购物

```javascript
pragma solidity ^0.4.0;

contract Purchase {
    uint public value;
    address public seller;
    address public buyer;
    enum State { Created, Locked, Inactive }
    State public state;

    function Purchase() payable {
        seller = msg.sender;
        value = msg.value / 2;
        if (2 * value != msg.value) throw;
    }

    modifier require(bool _condition) {
        if (!_condition) throw;
        _;
    }

    modifier onlyBuyer() {
        if (msg.sender != buyer) throw;
        _;
    }

    modifier onlySeller() {
        if (msg.sender != seller) throw;
        _;
    }

    modifier inState(State _state) {
        if (state != _state) throw;
        _;
    }

    event aborted();
    event purchaseConfirmed();
    event itemReceived();

    /// 终止购物并收回以太。仅仅可以在合约未锁定时被卖家调用。
    function abort()
        onlySeller
        inState(State.Created)
    {
        aborted();
        state = State.Inactive;
        if (!seller.send(this.balance))
            throw;
    }

    /// 买家确认购买。交易包含两倍价值的（`2 * value`）以太。
    /// 这些以太会一直锁定到收货确认(confirmReceived)被调用。
    function confirmPurchase()
        inState(State.Created)
        require(msg.value == 2 * value)
        payable
    {
        purchaseConfirmed();
        buyer = msg.sender;
        state = State.Locked;
    }

    /// 确认你（买家）收到了货物，这将释放锁定的以太。
    function confirmReceived()
        onlyBuyer
        inState(State.Locked)
    {
        itemReceived();
        // 首先改变状态是重要的
        // 否则下面会再次使用`send`调用合约
        state = State.Inactive;
        // 这里实际上允许买家和卖家阻止退款
        if (!buyer.send(value) || !seller.send(this.balance))
            throw;
    }
}
```