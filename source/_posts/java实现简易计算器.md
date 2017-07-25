---
title: java实现简易计算器
date: 2015-06-10 19:48:12
tags: java
category: java
---


一个简易计算器的java实现，采用的只是最基础的jbutton组件和相对应得监听事件以及BorderLayout和GridLayout两种布局方式。

    
    
    
```JAVA 
    package com.phoenix;
    import java.awt.BorderLayout;
    import java.awt.GridLayout;
    import java.awt.event.ActionEvent;
    import java.awt.event.ActionListener;
    
    import javax.swing.JButton;
    import javax.swing.JFrame;
    import javax.swing.JPanel;
    
    @SuppressWarnings("serial")
    public class CaculatorPanel extends JPanel{
    	private JButton display;
    	private JPanel panel;
    	private double result;
    	private String lastCommand;
    	private boolean start;
    	
    	public CaculatorPanel() {
    		setLayout(new BorderLayout());
    		result = 0;
    		lastCommand = "=";
    		start = true;
    		// add the display
    		display = new JButton("0");
    		display.setEnabled(false);
    		add(display, BorderLayout.NORTH);
 <!-- more -->   		
    		ActionListener insert = new InsertAction();
    		ActionListener command = new CommandAction();
    		
    		// add the buttons in a 4 * 4 grid
    		panel = new JPanel();
    		panel.setLayout(new GridLayout(4, 4));
    		
    		addButtons("7", insert);
    		addButtons("8", insert);
    		addButtons("9", insert);
    		addButtons("/", command);
    		
    		addButtons("4", insert);
    		addButtons("5", insert);
    		addButtons("6", insert);
    		addButtons("*", command);
    		
    		addButtons("1", insert);
    		addButtons("2", insert);
    		addButtons("3", insert);
    		addButtons("-", command);
    		
    		addButtons("0", insert);
    		addButtons(".", insert);
    		addButtons("=", command);
    		addButtons("+", command);
    		
    		add(panel, BorderLayout.CENTER);
    		
    	}
    	
    	private void addButtons(String lable, ActionListener listener) {
    		JButton button = new JButton(lable);
    		button.addActionListener(listener);
    		panel.add(button);
    	}
    	
    	private class InsertAction implements ActionListener {
    
    		@Override
    		public void actionPerformed(ActionEvent e) {
    			String input = e.getActionCommand();
    			if (start) {
    				display.setText("");
    				start = false;
    			}
    			display.setText(display.getText() + input);
    		}	
    	}
    	
    	
    	private class CommandAction implements ActionListener {
    
    		@Override
    		public void actionPerformed(ActionEvent e) {
    			String command = e.getActionCommand();
    			
    			if (start) {
    				if (command.equals("-")) {
    					display.setText(command);
    					start = false;
    				}else {
    					lastCommand = command;
    				}
    			}else {
    				caculate(Double.parseDouble(display.getText()));
    				lastCommand = command;
    				start = true;
    			}
    		}
    	}
    	
    	public void caculate(double x) {
    		if (lastCommand.equals("+")) {
    			result += x;
    		}else if (lastCommand.equals("-")) {
    			result -= x;
    		}else if (lastCommand.equals("*")) {
    			result *= x;
    		}else if (lastCommand.equals("/")) {
    			result /= x;
    		}else if (lastCommand.equals("=")) {
    			result = x;
    		}
    		display.setText("" + result);
    	}
    }
```