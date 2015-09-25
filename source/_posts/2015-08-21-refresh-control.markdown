---
layout: post
title: "UITableView 下拉更新"
date: 2015-08-21 14:58:22 +0800
comments: true
categories: iOS UITableView
---
###前言
許多App有下拉選單,即可更新的功能.

自己使用TableView想要達到類似功能,google了一下,做了demo.

<!--more-->

###實作
使用RefreshControl即可達成目的.

首先在viewWillApper裡面加入下列code,

baclgroundColor為"上方更新畫面"的背景顏色,
tintColor為 旋轉圈圈的顏色,
最後當下拉的時候會呼叫relodData()

{%codeblock TableViewController.m lang:objc %}
-(void)viewWillAppear:(BOOL)animated
{
    [super viewWillAppear:animated];
    
    [self reloadData];
    
    self.refreshControl = [[UIRefreshControl alloc] init];
    self.refreshControl.backgroundColor = [UIColor whiteColor];
    self.refreshControl.tintColor = [UIColor blackColor];
    [self.refreshControl addTarget:self
                            action:@selector(reloadData)
                  forControlEvents:UIControlEventValueChanged];
}
{%endcodeblock %}

在更新畫面中,除了轉圈之外,還有顯示更新的日期時間.

更新完成之後,記得加上endRefreshing結束刷新畫面.  

	[self.refreshControl endRefreshing];

{%codeblock TableViewController.m lang:objc %}
-(void)reloadData
{
    //載入資料
        
    if(self.refreshControl) {
        NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
        [formatter setDateFormat:@"MMM d, h:mm:ss a"];
        
        NSString *title = [NSString stringWithFormat:@"Last update: %@", [formatter stringFromDate:[NSDate date]]];
        NSDictionary *attrsDictionary = [NSDictionary dictionaryWithObject:[UIColor blackColor]
                                                                    forKey:NSForegroundColorAttributeName];
        
        NSAttributedString *attributedTitle = [[NSAttributedString alloc] initWithString:title attributes:attrsDictionary];
        
        self.refreshControl.attributedTitle = attributedTitle;
        
        [self.refreshControl endRefreshing];
    }    
}
{%endcodeblock %}  


###執行結果如下: 

{% img /images//2015/08/20150821_1.png %}

###參考資料
<a href=http://www.appcoda.com/pull-to-refresh-uitableview-empty/>Pull to refresh uitableviw</a>

<a href=https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIRefreshControl_class/>官方文件</a>