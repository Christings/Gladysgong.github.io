---
title: SMTP发送邮件
date: 2018-07-27 14:37:07
tags: Mail
categories:
		- Python
		- Mail
---
## 一.概述
	SMTP（Simple Mail Transfer Protocol）即简单邮件传输协议,它是一组用于由源地址到目的地址传送邮
	件的规则，由它来控制信件的中转方式。
	python中smtplib对smtp协议进行了简单封装，让我们能够方便的发送电子邮件，可以发送纯文本邮件、html
	邮件及带附件的邮件。
	python的SMTP支持smtplib和email两个模块，email负责构造邮件，smtplib负责发送邮件。
	
	email:
	
	from emial.mime.text import MIMEText
	msg=MIMEText('hello','plain','utf-8')
	注意：MIMEText对象第一个参数是邮件正文，第二个参数时MIME的subtype，传入'plain'，最终的MIME就是
	'text/plain'，最后用'utf-8'保证多语言兼容性。
	
	smtplib:
	
	import smtplib
	smtpobj=smtplib.SMTP([host[,port[,local_hostname]]])
	
	host: SMTP服务器主机,可指定主机的ip地址或者域名,可选参数;
	port: 如果你提供了host参数, 你需要指定SMTP服务使用的端口号，一般情况下SMTP端口号为25;
	local_hostname: 如果SMTP在你的本机上，你只需要指定服务器地址为localhost即可;
	
	# 发送邮件
	SMTP.sendmail(from_addr,to_addrs,msg[,mail_options,rcpt_options])
	
	from_addr: 邮件发送者地址；
	to_addrs: 字符串列表，邮件发送地址；
	msg: 发送消息；
	
## 二.代码实现
	# -*- coding: UTF-8 -*-
	''''
	title:自动发送测试
	author:Lilly
	'''
	import sys, os, re, urllib, urllib.parse
	import smtplib
	import traceback
	from email.mime.text import MIMEText
	from email.mime.multipart import MIMEMultipart
	
	
	def sendmail(subject, msg, toaddrs, fromaddr, smtpaddr, password):
	    '''''
	    @subject:邮件主题
	    @msg:邮件内容
	    @toaddrs:收信人的邮箱地址
	    @fromaddr:发信人的邮箱地址
	    @smtpaddr:smtp服务地址，可以在邮箱看，比如163邮箱为smtp.163.com
	    @password:发信人的邮箱密码
	    '''
	    mail_msg = MIMEMultipart()
	    if not isinstance(subject, str):
	        subject = str(subject, 'utf-8')
	    mail_msg['Subject'] = subject
	    mail_msg['From'] = fromaddr
	    mail_msg['To'] = ','.join(toaddrs)
	    mail_msg.attach(MIMEText(msg, 'html', 'utf-8'))
	    try:
	        s = smtplib.SMTP()
	        s.connect(smtpaddr)  # 连接smtp服务器
	        s.set_debuglevel(1)  # 打印和SMTP服务交互的所有信息
	        s.login(fromaddr, password)  # 登录邮箱
	        s.sendmail(fromaddr, toaddrs, mail_msg.as_string())  # 发送邮件
	        s.quit()
	    except Exception as e:
	        print("Error: unable to send email")
	        print(traceback.format_exc())
	
	
	if __name__ == '__main__':
	    fromaddr = "voidqueen@163.com"
	    smtpaddr = "smtp.163.com"
	    toaddrs = ["531404376@qq.com"]
	    subject = "测试邮件"
	    password = "********" # 邮箱客户端授权码
	    msg = "测试测试"
	    sendmail(subject, msg, toaddrs, fromaddr, smtpaddr, password)
## 三.小结
	构造一个邮件对象就是一个Messag对象；
	如果构造一个MIMEText对象，就表示一个文本邮件对象；
	如果构造一个MIMEImage对象，就表示一个作为附件的图片；
	要把多个对象组合起来，就用MIMEMultipart对象；
	而MIMEBase可以表示任何对象。
	它们的继承关系如下：
	Message
	+- MIMEBase
	   +- MIMEMultipart
	   +- MIMENonMultipart
	      +- MIMEMessage
	      +- MIMEText
	      +- MIMEImage
	      
## 四.参考文档

[email.mime参考文档](https://docs.python.org/2/library/email.mime.html)

[廖雪峰-SMTP发送邮件](https://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/001386832745198026a685614e7462fb57dbf733cc9f3ad000)
	      

