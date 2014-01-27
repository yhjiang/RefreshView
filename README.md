### MJRefreshView

 
 作者说：网上开源的下拉-上拉刷新控件，普遍封装得过于复杂、耦合性强。因此本人特地花了点时间写了一套无耦合、可插拔式的刷新控件，对项目中的其他代码毫无侵入性，而且使用简单，3行代码就能集成刷新控件。 

###Usage:
```  
//添加下拉刷新 
    _header = [[MJRefreshHeaderView alloc] init]; 
    _header.delegate = self; 
    _header.scrollView = self.tableView; 
     
//添加上拉加载更多 
    _footer = [[MJRefreshFooterView alloc] init]; 
    _footer.delegate = self; 
    _footer.scrollView = self.tableView; 
```  
###Delegate
```  
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
```  

###Tips

 1. 监听刷新控件的状态有2种方式：
 * 设置delegate，通过代理方法监听(参考MJCollectionViewController.m)
 * 设置block，通过block回调监听(参考MJTableViewController.m)
 
 2. 可以在MJRefreshConst.h和MJRefreshConst.m文件中自定义显示的文字内容和文字颜色
 
 3. 本框架兼容iOS6\iOS7，iPhone\iPad横竖屏
 
 4.为了保证内部不泄露，最好在控制器的dealloc中释放占用的内存
    - (void)dealloc
    {
        [_header free];
        [_footer free];
    }
 
 5.自动刷新：调用beginRefreshing可以自动进入下拉刷新状态
 
 6.结束刷新
 1> endRefreshing
 2> endRefreshingWithoutIdle(在endRefreshing不好使的情况下，才调用这个方法)


Copyright (c)  感谢原作者新浪微博@M了个J的无私奉献。
