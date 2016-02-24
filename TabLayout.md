#android support design之TabLayout实现顶部或底部导航#

顶部导航或底部导航实现有很多种方式，但android support design中提供了一种很简便的实现方式，

	tabLayout.setupWithViewPager(viewPager);

一句话搞定所有，实现底部（顶部）导航切换，支持左右滑动，导航按钮自动选中功能。直接看代码

<pre><code>//CCViewPagerActivity
package com.android.cc.cc_android.viewPager;

import android.os.Bundle;
import android.support.design.widget.TabLayout;
import android.support.v4.app.Fragment;
import android.support.v4.app.FragmentManager;
import android.support.v4.app.FragmentStatePagerAdapter;
import android.support.v4.app.ListFragment;
import android.support.v4.view.ViewPager;
import android.support.v7.app.AppCompatActivity;
import android.widget.TableLayout;


import com.android.cc.cc_android.R;

import java.util.ArrayList;
import java.util.List;

/**
 * Created by chenchao on 16/1/3.
 */
public class CCViewPagerActivity extends AppCompatActivity {

    MyAdapter mAdapter = new MyAdapter(getSupportFragmentManager());
    @Override
    protected  void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        setContentView(R.layout.viewpager_layout);

        TabLayout tabLayout = (TabLayout)findViewById(R.id.tabs);

        ViewPager viewPager = (ViewPager)findViewById(R.id.viewPager);

        viewPager.setAdapter(mAdapter);
        tabLayout.setTabMode(TabLayout.MODE_FIXED);
        tabLayout.setTabGravity(TabLayout.GRAVITY_FILL);

        tabLayout.setupWithViewPager(viewPager);
        viewPager.setCurrentItem(0);
    }

    public  static  class MyAdapter extends FragmentStatePagerAdapter {
        List<Fragment> mFragments = new ArrayList<>();

        public MyAdapter(FragmentManager fm) {
            super(fm);
            mFragments.add(new ListFragment());
            mFragments.add(new ListFragment());
        }

        @Override
        public int getCount() {
            return mFragments.size();
        }

        @Override
        public CharSequence getPageTitle(int position) {
            if (position == 0) {
                return "test1";
            } else {
                return "test2";
            }
        }

        @Override
        public Fragment getItem(int position) {
            if (position < mFragments.size()) {
                return mFragments.get(position);
            }

            return null;
        }
    }

}

</pre></code>

	<?xml version="1.0" encoding="utf-8"?>
	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
   		xmlns:app="http://schemas.android.com/apk/res-auto"
    	android:layout_width="match_parent"
    	android:fitsSystemWindows="true"
    	android:layout_height="match_parent">

    	<android.support.design.widget.AppBarLayout
        	android:layout_width="match_parent"
        	android:layout_height="wrap_content"
        	android:theme="@style/AppTheme.AppBarOverlay" >

        	<android.support.design.widget.TabLayout
            	android:id="@+id/tabs"
            	app:tabSelectedTextColor="@android:color/holo_blue_bright"
            	app:tabTextColor="@android:color/black"
            	app:tabIndicatorColor="@android:color/holo_blue_bright"
            	android:layout_width="match_parent"
            	android:layout_height="wrap_content"
           		android:layout_alignParentTop="true"
            	android:background="#edf0f9" />
    		</android.support.design.widget.AppBarLayout>

    	<!--可滑动的布局内容-->
    	<android.support.v4.view.ViewPager
        	android:id="@+id/viewPager"
        	android:layout_width="match_parent"
        	android:layout_height="match_parent"
        	app:layout_behavior="@string/appbar_scrolling_view_behavior"
        	android:layout_below="@+id/tabs" />

	</RelativeLayout>


    
