#scrollView的使用


###scrollView垂直滚动的情况下 通过加速度判断是向上还是向下滚动###

向上为正， 向下为负，为0 没有速度（慢速拖动）


	-(void)scrollViewWillEndDragging:(UIScrollView *)scrollView withVelocity:(CGPoint)velocity 	targetContentOffset:(inout CGPoint *)targetContentOffset
	{
	// called on finger up if the user dragged. decelerate is true if it will continue moving 	afterwards
	    NSLog(@"%@",NSStringFromCGPoint(velocity));
	}



###方法2： 判断scrollView是向上移动还是向下移动

	@interface ViewController ()<UIScrollViewDelegate>
	@property (strong, nonatomic) IBOutlet UIScrollView *scrollView;

	@end

	@implementation ViewController
	{
    CGFloat _oldy;
	}
	- (void)viewDidLoad {
    [super viewDidLoad];
    self.scrollView.delegate = self;
    self.scrollView.backgroundColor = [UIColor redColor];
    self.scrollView.contentSize = CGSizeMake(self.view.frame.size.width, 	CGRectGetHeight(self.view.frame)*2);
	}

	- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
	}

	-(void)scrollViewWillBeginDragging:(UIScrollView *)scrollView
	{
	    
	    _oldy = scrollView.contentOffset.y;
		//    NSLog(@"scrollView begin dragging");
	}
	
	-(void)scrollViewDidScroll:(UIScrollView *)scrollView
	{
	    CGFloat _new_y = scrollView.contentOffset.y;
	//    NSLog(@"%f",    );
	    if(_oldy < _new_y) {
	        NSLog(@"向下移动");
	    }
	    else {
	        NSLog(@"向上移动");
	    }
	    _oldy = _new_y;
	}
	@end
	
###方法3：[如何关闭scrollview的滚动后的减速度](http://stackoverflow.com/questions/1105647/deactivate-uiscrollview-decelerating)###

	-(void)scrollViewWillBeginDecelerating:(UIScrollView *)scrollView{  
    	[scrollView setContentOffset:scrollView.contentOffset animated:YES];   
    }


###方法4：[限制scrollView的bounce 高度](http://stackoverflow.com/questions/13842427/limit-bouncing-for-uiscrollview-in-ios)

	- (void)scrollViewDidScroll:(UIScrollView *)scrollView {
    	if (scrollView.contentOffset.y < -20) {
        	scrollView.contentOffset = CGPointMake(0, -20);
    	}
	}