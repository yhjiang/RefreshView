 作者说：网上开源的下拉-上拉刷新控件，普遍封装得过于复杂、耦合性强。因此本人特地花了点时间写了一套无耦合、可插拔式的刷新控件，对项目中的其他代码毫无侵入性，而且使用简单，3行代码就能集成刷新控件。 

使用方法：

//添加下拉刷新 
    _header = [[MJRefreshHeaderView alloc] init]; 
    _header.delegate = self; 
    _header.scrollView = self.tableView; 
     
//添加上拉加载更多 
    _footer = [[MJRefreshFooterView alloc] init]; 
    _footer.delegate = self; 
    _footer.scrollView = self.tableView; 

在代理行数实现刷新功能： 
#pragma mark 代理方法-进入刷新状态就会调用 
- (void)refreshViewBeginRefreshing:(MJRefreshBaseView *)refreshView 
{ 
    NSDateFormatter *formatter = [[NSDateFormatter alloc] init]; 
    formatter.dateFormat = @"HH : mm : ss.SSS"; 
    if (_header == refreshView) { 
        for (int i = 0; i<5; i++) { 
            [_data insertObject:[formatter stringFromDate:[NSDate date]] atIndex:0]; 
        } 
         
    } else { 
        for (int i = 0; i<5; i++) { 
            [_data addObject:[formatter stringFromDate:[NSDate date]]]; 
        } 
    } 
    [NSTimer scheduledTimerWithTimeInterval:1 target:self.tableView selector:@selector(reloadData) userInfo:nil repeats:NO]; 
}



Copyright (c)  感谢原作者新浪微博@M了个J的无私奉献。
