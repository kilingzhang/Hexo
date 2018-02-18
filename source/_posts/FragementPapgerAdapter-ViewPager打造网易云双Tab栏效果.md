---
title: FragementPapgerAdapter-ViewPager打造网易云双Tab栏效果
date: 2017-04-29 12:04:39
tags: [Android, NeteaseCloudMusic, 网易云音乐]
top: 1000
---
今天我们记录的是我用`FragementPagerAdapter+ViewPager`打造网易云双Tab栏的效果，我们先看下效果图(图片资源较大,耐心等待,额,第一次这么正式的写博客,没仔细研究怎么搞这些图片的处理.)：    
![](http://www.kilingzhang.com/img/2017-04-08_00_54_46.gif)



<!--more-->

先说点题外话,安卓的入门小白，为了接手部门的拱拱app。我硬生生的从后端转来做`Android`(好吧,是我自己想转的. 咳咳)。啃了一下郭神的第一行代码，又从慕课中接触到了hongyang大大的视频。受益颇多。无论学习什么语言，我觉得实战是最重要的，所以在学习的过程中，我向网易云音乐下了黑手。我准备在入门阶段高仿一个网易云音乐app。我在github上看到了有一个3000+star且还在增长的一个同是仿写网易云音乐的一个项目。不过实话实说我这个人比较强迫症，他的api用的是百度，而且UI仿照的也只是形似但是很多网易云本身自带的许多效果都不具备。所以我准备自己动手写一个自己心目中的高仿网易云app。当然在写的过程中我也在虚心学习刚才提到的大大的代码。取长补短嘛。没有冒犯的意思，只是个人强迫症加上入门练手。额，言归正传。

好今天我们写的就是上图的Tab滚动,在慕课网中看了hongyang大大的 [多种多样的App主界面Tab实现方法](http://www.imooc.com/learn/264)一课.我接触到了制作Tab选项卡的四种实现方式:


1. `ViewPager`     
2. `Fragement`       
3. `FragementVeiwAdapter`+`ViewPager`     
4. `ViewPagerIndicator`+`ViewPager`    


简单说下这四种方式的优劣,具体可以去看hongyang大神的视频:

1. 第一种实现简单,但是现在已经不被谷歌官方所推荐,而且代码维护性差,因为所有的代码都要在MainActivity中去操作。所以不推荐使用.  
2. 第二种和第三种是官方推荐使用的一种实现方式,但是第二种和第三种存在一个明显区别就是第二种方式不可以实现滑动翻页,也就是说效果类似QQ的Tab切换 只可以点击切换不可以滑动切换.而第三种则均可实现.而且这两种方法实现有一个最大的好处就是利于代码维护.因为这里的MainActivity只是负责初始化`Fragement`具体每个`Fragement`里面的操作都是每个`Fragement`类自己实现管理的.所以代码的可维护性非常高.也越来越被官方所推荐.
3. 第四种则是一个github上的一个开源项目.功能挺强大的.但是我并没有深入的了解.因为暂时觉得还用不上那么强大的控件.以后可以用来研究一下源码.看看大神们怎么实现的用来学习提高.

所以我选择了第三种实现方式去实现网易云的这个双层Tab滚动,实现后的效果如下:        
![](http://www.kilingzhang.com/img/2017-04-08_01_42_31.gif)   

看到效果图,我们先分析下这个效果是怎么实现的:          
首先最上层我们看到从本地音乐tab一直滚动到社区tab.在滚动到在线音乐tab时,在线音乐tab中又有四个小tab. 所以这很容易让我们联想到双层Tab的滚动. 所以我们实现这个效果的方法也是分两层进行实现的.

###首先先实现第一层的Tab滚动

- 首先我们先实现MainActivity布局文件             



		<?xml version="1.0" encoding="utf-8"?>
	    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
		    xmlns:app="http://schemas.android.com/apk/res-auto"
		    android:id="@+id/id_linear_layout"
		    android:layout_width="match_parent"
		    android:layout_height="match_parent"
		    	android:orientation="vertical">
	    
		    <LinearLayout
			    android:id="@+id/id_top_linearLayout"
			    android:layout_width="170dp"
			    android:layout_height="40dp"
			    android:layout_gravity="center"
			    android:background="#CC3300"
			    android:gravity="center"
			    android:orientation="horizontal">
			    
			    <ImageView
				    android:id="@+id/id_top_linearLayout_music_icon"
				    android:layout_width="0dp"
				    android:layout_height="wrap_content"
				    android:layout_weight="1"
				    android:src="@drawable/ic_music_default" />
			    
			    <ImageView
			    android:id="@+id/id_top_linearLayout_NetEasemusic_icon"
				    android:layout_width="0dp"
				    android:layout_height="wrap_content"
				    android:layout_weight="1"
				    android:src="@drawable/ic_net_ease_music_press" />
				    
			    <ImageView
				    android:id="@+id/id_top_linearLayout_group_icon"
				    android:layout_width="0dp"
				    android:layout_height="wrap_content"
				    android:layout_weight="1"
				    android:src="@drawable/ic_group_default" />
		    
		    </LinearLayout>
		    
		    <android.support.v4.view.ViewPager
			    android:id="@+id/id_top_veiwPager"
			    android:layout_width="match_parent"
			    android:layout_height="wrap_content">
			</android.support.v4.view.ViewPager>
	       
	    </LinearLayout>



- 然后我们创建我们的三个Fragement类的布局

我们创建依次创建三个`MyMusicTabFragement,NetEaseCloudMusicTabFrangement,GroupTabFrangement`三个`Frangement`类的布局文件
这里我们以`NetEaseCloudMusicTabFrangement`为例 因为下文中要继续用到这个的布局   


	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:orientation="vertical">
	    <TextView
	        android:layout_width="match_parent"
	        android:layout_height="match_parent"
	        android:gravity="center"
	        android:text="This is NetEaseCloudMusic" />
	</LinearLayout>



- 创建`Fragement`类将类与布局进行绑定

然后我们创建`MyMusicTabFragement`类文件. 我们先将刚刚从创建的布局文件和`MyMusicTabFragement`类进行绑定. 



		package com.kilingzhang.neteasemusictab.Fragement;
		
		import android.os.Bundle;
		import android.support.annotation.Nullable;
		import android.support.v4.app.Fragment;
		import android.view.LayoutInflater;
		import android.view.View;
		import android.view.ViewGroup;

		import com.kilingzhang.neteasemusictab.R;
		
		/**
		 * Created by Slight on 2017/4/9.
		 */
		
		public class NetEaseCloudMusic extends Fragment {
		    /**
		     *  这里一定要注意一点,在引入Fragemnt的时候 一定要注意引入的是V4包还是v7包
		     *  V4包向下兼容好些 但是无论引入哪个包 一定要记得在本项目中所有的包都要引入相同的包
		     *  否则会抱错哦  这里hongyang大大也已经强调过了
		     */
		
		    @Nullable
		    @Override
		    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
		        //获取MyMusicTabFragemnt布局
		        return inflater.inflate(R.layout.neteasecloudmusic_tab_fragement,container,false);
		    }
		}




- 重复上述步骤我们把三个Tab的类和布局文件都建好. 

暂时省略重复过程. 最后会提供全部源码   
             
- 在MainActivity中初始化控件

到现在我们现在已经准备好第一层tab的所有的布局文件和绑定类文件.下面我们来到`MainActivity`中 初始化稍后我们需要用到的控件. 这里我们创建个初始化控件的方法 .方便以后调用 以增加代码可读性和方便后期维护.



		package com.kilingzhang.neteasemusictab.Activity;
		
		import android.support.v4.app.FragmentPagerAdapter;
		import android.support.v4.view.ViewPager;
		import android.support.v7.app.AppCompatActivity;
		import android.os.Bundle;
		import com.kilingzhang.neteasemusictab.Fragement.GroupTabFragement;
		import com.kilingzhang.neteasemusictab.Fragement.MyMusicTabFragement;
		import com.kilingzhang.neteasemusictab.Fragement.NetEaseCloudMusicTabFragement;

		import com.kilingzhang.neteasemusictab.R;
		
		public class MainActivity extends AppCompatActivity {
		
		    private ImageView myMusic;
		    private ImageView netEaseMusic;
		    private ImageView group;


		    private ViewPager mViewPager;
		    private FragmentPagerAdapter mFragmentPagerAdapter;
		    private MyMusicTabFragement myMusicTabFragement;
		    private NetEaseCloudMusicTabFragement netEaseCloudMusicTabFragement;
		    private GroupTabFragement groupTabFragement;
			创建List用来存储适配器所需要的数据
			private List<Fragment> mFragements = new ArrayList<Fragment>();
			
		    @Override
		    protected void onCreate(Bundle savedInstanceState) {
		        super.onCreate(savedInstanceState);
		        setContentView(R.layout.activity_main);
		        ininViews();
		    }
		
		    private void ininViews() {
		        mViewPager = (ViewPager) findViewById(R.id.id_top_veiwPager);
		        myMusic = (ImageView) findViewById(R.id.id_top_linearLayout_music_icon);
		        netEaseMusic = (ImageView) findViewById(R.id.id_top_linearLayout_NetEasemusic_icon);
		        group = (ImageView) findViewById(R.id.id_top_linearLayout_group_icon);

		        myMusicTabFragement = new MyMusicTabFragement();
		        netEaseCloudMusicTabFragement = new NetEaseCloudMusicTabFragement();
		        groupTabFragement = new GroupTabFragement();
		 		//将我们初始化的数据存放到List中 稍后传给适配器用于构件ViewPager中的视图
		        mFragements.add(myMusicTabFragement);
		        mFragements.add(netEaseCloudMusicTabFragement);
		        mFragements.add(groupTabFragement);
				
		    }
		}




- 创建FragementAdapter并将Adapter与ViewPager绑定



	    //这里我们要注意  如果我们Fragement导入的是app包 我们获取FragmentManager要用getFragmentManager()
        mFragmentPagerAdapter = new FragmentPagerAdapter(getSupportFragmentManager()) {
            @Override
            public Fragment getItem(int position) {
                return mFragements.get(position);
            }

            @Override
            public int getCount() {
                return mFragements.size();
            }
        };
        mViewPager.setAdapter(mFragmentPagerAdapter);


- 查看效果


- 添加滑动点击后Tab的icon效果改变

好,我们现在已经完成了第一层的效果. 如图所示, 但是我们发现. 这跟我们想要实现的效果不太一样啊 ,很多效果都没实现出来 .不能点击tab跳转Tab.跳转tab后 icon的状态不会改变 . 是的 ,这些效果都是要们后期自己实现的 .这样也就可以实现自己的想法了. 好下面我们开始按照网易云的效果完善效果.    
		


		@Override
	    protected void onCreate(Bundle savedInstanceState) {
	        super.onCreate(savedInstanceState);
	        setContentView(R.layout.activity_main);
	        //初始化控件
	        ininViews();
	        //初始化控件事件
	        ininEvents();
	        //初始化默认显示tab
	        setViewItem(1);
	    }
	
	    private void ininEvents() {
	        myMusic.setOnClickListener(this);
	        netEaseMusic.setOnClickListener(this);
	        group.setOnClickListener(this);
	        mViewPager.setOnPageChangeListener(new ViewPager.OnPageChangeListener() {
	            //这是ViewPager的滑动时触发的事件
	            @Override
	            public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {
	
	            }
	            //ViewPager被选择后触发的事件
	            @Override
	            public void onPageSelected(int position) {
	                setViewItem(position);
	            }
	            //ViewPager的状态
	            @Override
	            public void onPageScrollStateChanged(int state) {
	
	            }
	        });
	    }
	    
	    //每次选择Item时候,为了方便代码,先将所有icon设置成默认状态
	    private void initIcon() {
	        myMusic.setImageResource(R.drawable.ic_music_default);
	        netEaseMusic.setImageResource(R.drawable.ic_net_ease_music_default);
	        group.setImageResource(R.drawable.ic_group_default);
	    }
	
	    //封装设置Item的方法 方便调用和代码维护
	    private void setViewItem(int postion) {
	        //现初始化icon
	        initIcon();
	        switch (postion) {
	            case 0:
	                //设置选中状态
	                myMusic.setImageResource(R.drawable.ic_music_press);
	                //设置ViewPager需要现实的Item
	                mViewPager.setCurrentItem(0);
	                break;
	            case 1:
	                netEaseMusic.setImageResource(R.drawable.ic_net_ease_music_press);
	                mViewPager.setCurrentItem(1);
	                break;
	            case 2:
	                group.setImageResource(R.drawable.ic_group_press);
	                mViewPager.setCurrentItem(2);
	                break;
	            default:
	                break;
	        }
	    }
	
	    @Override
	    public void onClick(View view) {
	        //设置点击跳转事件
	        switch (view.getId()) {
	            case R.id.id_top_linearLayout_music_icon:
	                setViewItem(0);
	                break;
	            case R.id.id_top_linearLayout_NetEasemusic_icon:
	                setViewItem(1);
	                break;
	            case R.id.id_top_linearLayout_group_icon:
	                setViewItem(2);
	                break;
	            default:
	                break;
	        }
	    }



- 第一层完成 

我们来看下效果:




###首先先实现第二层的Tab滚动

我们完成了这个Demo中的第一层Tab切换,以上的内容都是hongyang大大在视频中讲过的知识点. 没有什么特别说的. 接下来就要到重点的问题上来了.我们如何实现这个第二层的Tab切换.首先我们重新分析下我们的这个Demo的效果和布局.首先他的全局效果是.我们可以从最上层的Tab一路滑动到最后一个Tab中.并且在第二个NetEaseCloudMusicTab中时. 继续滑动切换的是NetEaseCloudMusicTab内部的四个Tab.当这四个Tab滑动完之后.再滑动到左后一个Tab中 .反之同理. 所以,我们很容易想到用两层的Fragement嵌套做出双层Tab的效果.

- 在Net_Ease_cloud 布局文件中实现第二层的布局

代码




	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:orientation="vertical">
	    <LinearLayout
	        android:layout_width="match_parent"
	        android:layout_height="40dp"
	        android:orientation="vertical">
	        <LinearLayout
	            android:layout_width="match_parent"
	            android:layout_height="0dp"
	            android:layout_weight="1"
	            android:orientation="horizontal">
	            <TextView
	                android:id="@+id/id_recommed_tab"
	                android:layout_width="0dp"
	                android:layout_height="match_parent"
	                android:layout_weight="1"
	                android:gravity="center"
	                android:text="个性推荐"/>
	            <TextView
	                android:id="@+id/id_songlist_tab"
	                android:layout_width="0dp"
	                android:layout_height="match_parent"
	                android:layout_weight="1"
	                android:gravity="center"
	                android:text="歌单"/>
	            <TextView
	                android:id="@+id/id_radio_tab"
	                android:layout_width="0dp"
	                android:layout_height="match_parent"
	                android:layout_weight="1"
	                android:gravity="center"
	                android:text="主播电台"/>
	            <TextView
	                android:id="@+id/id_rank_tab"
	                android:layout_width="0dp"
	                android:layout_height="match_parent"
	                android:layout_weight="1"
	                android:gravity="center"
	                android:text="排行榜"/>
	        </LinearLayout>
       		<!--用来实现Tab的游标,因为我们用的不是开源控件,所以我们要自己实现-->
	        <ImageView
	            android:id="@+id/id_tab_indicator"
	            android:layout_width="100dp"
	            android:layout_height="3dp"
	            android:background="#CC3300"/>
	    </LinearLayout>
	    <android.support.v4.view.ViewPager
	        android:id="@+id/id_neteaseCloud_viewpager"
	        android:layout_width="match_parent"
	        android:layout_height="0dp"
	        android:layout_weight="1"/>
	</LinearLayout>




- 然后我们创建我们第二层的四个Fragement类的布局

代码



	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:orientation="vertical">
	
	    <TextView
	        android:layout_width="match_parent"
	        android:layout_height="match_parent"
	        android:gravity="center"
	        android:text="This is Recommed" />
	</LinearLayout>



- 创建Fragement类将类与布局进行绑定

同理上文 不再赘述


- 在NetEaseCloudFragement中初始化控件 ,
- 创建FragementAdapter  将四个Fragement添加到适配器中
- ViewPager绑定适配器

由于大部分都是重复第一层Tab的操作,所以直接贴代码,这里唯一要注意的就是我们通过id找控件的findviewbyid这个函数找不到了.不能直接调用了.所以我们这里要先保存Fragement的view 然后通过view来进行找 




	package com.kilingzhang.neteasemusictab.Fragement;
	
	import android.content.Context;
	import android.os.Bundle;
	import android.support.annotation.Nullable;
	import android.support.v4.app.Fragment;
	import android.support.v4.app.FragmentPagerAdapter;
	import android.support.v4.view.ViewPager;
	import android.view.LayoutInflater;
	import android.view.View;
	import android.view.ViewGroup;
	import android.widget.ImageView;
	import android.widget.TextView;
	
	import com.kilingzhang.neteasemusictab.R;
	
	import java.util.ArrayList;
	import java.util.List;
	
	/**
	 * Created by Slight on 2017/4/9.
	 */
	
	public class NetEaseCloudMusicTabFragement extends Fragment  implements View.OnClickListener{
	    /**
	     *  这里一定要注意一点,在引入Fragemnt的时候 一定要注意引入的是V4包还是app包
	     *  V4包向下兼容好些 但是无论引入哪个包 一定要记得在本项目中所有的包都要引入相同的包
	     *  否则会抱错哦  这里hongyang大大也已经强调过了
	     */
	
	    private Fragment recommedFragement;
	    private Fragment songlistFragement;
	    private Fragment radioFragement;
	    private Fragment ranklistFragement;
	
	    private TextView recommedTab;
	    private TextView songlistTab;
	    private TextView radioTab;
	    private TextView ranklistTab;
	    private ImageView tabIndicator;
	    private View view;
	    private ViewPager viewPager;
	
	    private FragmentPagerAdapter mFragmentPagerAdapter;
	    private List<Fragment> mFragements = new ArrayList<Fragment>();
	
	    @Nullable
	    @Override
	    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
	        //获取MyMusicTabFragemnt布局
	        view = inflater.inflate(R.layout.neteasecloudmusic_tab_fragement,container,false);
	        ininViews(inflater,container);
	        ininEvents();
	        return  view;
	    }
	
	    private void ininEvents() {
	        recommedTab.setOnClickListener(this);
	        songlistTab.setOnClickListener(this);
	        radioTab.setOnClickListener(this);
	        ranklistTab.setOnClickListener(this);
	    }
	
	    private void ininViews(LayoutInflater inflater, @Nullable ViewGroup container) {
	        viewPager = (ViewPager) view.findViewById(R.id.id_neteaseCloud_viewpager);
	
	        tabIndicator = (ImageView) view.findViewById(R.id.id_tab_indicator);
	        recommedTab = (TextView) view.findViewById(R.id.id_recommed_tab);
	        songlistTab = (TextView) view.findViewById(R.id.id_songlist_tab);
	        radioTab = (TextView) view.findViewById(R.id.id_radio_tab);
	        ranklistTab = (TextView) view.findViewById(R.id.id_rank_tab);
	
	
	        recommedFragement = new RecommedTabFragement();
	        songlistFragement = new SonglistTabFragement();
	        radioFragement = new RadioTabFragement();
	        ranklistFragement = new RanklistTabFragement();
	        mFragements.add(recommedFragement);
	        mFragements.add(songlistFragement);
	        mFragements.add(radioFragement);
	        mFragements.add(ranklistFragement);
	
	        mFragmentPagerAdapter = new FragmentPagerAdapter(getFragmentManager()) {
	            @Override
	            public Fragment getItem(int position) {
	                return mFragements.get(position);
	            }
	
	            @Override
	            public int getCount() {
	                return mFragements.size();
	            }
	        };
	
	        viewPager.setAdapter(mFragmentPagerAdapter);
	
	
	    }
	
	    @Override
	    public void onClick(View view) {
	        switch (view.getId()){
	            case R.id.id_recommed_tab:
	
	                break;
	            case R.id.id_songlist_tab:
	
	                break;
	            case R.id.id_radio_tab:
	
	                break;
	            case R.id.id_rank_tab:
	
	                break;
	            default:
	                break;
	        }
	    }
	}


- 查看效果

- 添加滑动点击后Tab的icon效果改变

首先要在MainActivity中初始化屏幕宽度


	
	private void initScreen() {
        DisplayMetrics outMetrics = new DisplayMetrics();
        display = getWindow().getWindowManager().getDefaultDisplay();
        display.getMetrics(outMetrics);
        mScreen = outMetrics.widthPixels;
    }




然后完成效果



	private void ininEvents() {
	    recommedTab.setOnClickListener(this);
	    songlistTab.setOnClickListener(this);
	    radioTab.setOnClickListener(this);
	    ranklistTab.setOnClickListener(this);
	    viewPager.setOnPageChangeListener(new ViewPager.OnPageChangeListener() {
	        @Override
	        public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {
	            LinearLayout.LayoutParams layoutParams = (LinearLayout.LayoutParams) tabIndicator.getLayoutParams();
	            if(mCurrentIndex == 0 && position == 0){
	                layoutParams.leftMargin = (int) ((mCurrentIndex) * mScreenTab + positionOffset * mScreenTab);
	            }else if(mCurrentIndex == 1 && position == 0){
	                layoutParams.leftMargin = (int) ((mCurrentIndex) * mScreenTab + (positionOffset -1) * mScreenTab);
	            }else if(mCurrentIndex == 1 && position == 1){
	                layoutParams.leftMargin = (int) ((mCurrentIndex) * mScreenTab + positionOffset * mScreenTab);
	            }else if(mCurrentIndex == 2 && position == 1){
	                layoutParams.leftMargin = (int) ((mCurrentIndex) * mScreenTab + (positionOffset -1) * mScreenTab);
	            }else if(mCurrentIndex == 2 && position == 2){
	                layoutParams.leftMargin = (int) ((mCurrentIndex) * mScreenTab + positionOffset * mScreenTab);
	            }else if(mCurrentIndex == 3 && position == 2){
	                layoutParams.leftMargin = (int) ((mCurrentIndex) * mScreenTab + (positionOffset -1) * mScreenTab);
	            }else if(mCurrentIndex == 3 && position == 3){
	                layoutParams.leftMargin = (int) ((mCurrentIndex) * mScreenTab + positionOffset * mScreenTab);
	            }
	            tabIndicator.setLayoutParams(layoutParams);
	        }
	
	        @Override
	        public void onPageSelected(int position) {
	            mCurrentIndex = position;
	        }
	
	        @Override
	        public void onPageScrollStateChanged(int state) {
	
	        }
	    });
	}
	
	private void initTabScreen() {
	    mScreenTab = MainActivity.mScreen / mFragements.size();
	    LinearLayout.LayoutParams layoutParams = (LinearLayout.LayoutParams) tabIndicator.getLayoutParams();
	    layoutParams.width = (int) mScreenTab;
	    tabIndicator.setLayoutParams(layoutParams);
	}
	
	@Override
	public void onClick(View view) {
	    switch (view.getId()){
	        case R.id.id_recommed_tab:
	            viewPager.setCurrentItem(0);
	            break;
	        case R.id.id_songlist_tab:
	            viewPager.setCurrentItem(1);
	            break;
	        case R.id.id_radio_tab:
	            viewPager.setCurrentItem(2);
	            break;
	        case R.id.id_rank_tab:
	            viewPager.setCurrentItem(3);
	            break;
	        default:
	            break;
	    }
	}	



	 

至此,我们的所有效果就完成了.