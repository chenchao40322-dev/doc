# android创建menu的几种方式 #
###创建menu的方式以下几种：###
- **Options Menu**
- **Floating Context Menu**
- **Contextual Action Modes**
- **PopupMenu**


## Options Menu ##
###创建选项菜单：重写 Activity的onCreateOptionsMenu()
    @Override
	public boolean onCreateOptionsMenu(Menu menu) {
    	MenuInflater inflater = getMenuInflater();
    	inflater.inflate(R.menu.game_menu, menu);
    	return true;
	}
###处理点击事件：
重写 Activity的onOptionsItemSelected()，此方法将传递所选的 MenuItem。您可以通过调用 getItemId() 方法来识别项目。   

    @Override
	public boolean onCreateOptionsMenu(Menu menu) {
    	MenuInflater inflater = getMenuInflater();
    	inflater.inflate(R.menu.game_menu, menu);
    	return true;
	}


## Floating Context Menu ##
###注册关联上下文菜单：
通过调用Activity的registerForContextMenu()，为对应的view注册上下文菜单，把上下文菜单和View关联起来，如果希望为 ListView 或 GridView的每个item均提供相同的上下文菜单，请通过将 ListView 或 GridView 传递给 registerForContextMenu()。
###创建浮动上下文菜单：
在 Activity 或 Fragment 中实现 onCreateContextMenu() 方法。
###处理点击事件：
实现 onContextItemSelected()。

	@Override
	public boolean onContextItemSelected(MenuItem item) {
    	AdapterContextMenuInfo info = (AdapterContextMenuInfo) item.getMenuInfo();
    	switch (item.getItemId()) {
        	case R.id.edit:
            	editNote(info.id);
            	return true;
        	case R.id.delete:
            	deleteNote(info.id);
            	return true;
        	default:
            	return super.onContextItemSelected(item);
    	}
	}


## Contextual Action Modes ##

屏幕顶部将出现一个“上下文操作栏”，显示用户可对当前所选项执行的操作。

**-为单个视图启用上下文操作模式：**

**----实现 ActionMode.Callback 接口：**

    private ActionMode.Callback mActionModeCallback = new ActionMode.Callback() {

    // Called when the action mode is created; startActionMode() was called
    @Override
    public boolean onCreateActionMode(ActionMode mode, Menu menu) {
        // Inflate a menu resource providing context menu items
        MenuInflater inflater = mode.getMenuInflater();
        inflater.inflate(R.menu.context_menu, menu);
        return true;
    }

    // Called each time the action mode is shown. Always called after onCreateActionMode, but
    // may be called multiple times if the mode is invalidated.
    @Override
    public boolean onPrepareActionMode(ActionMode mode, Menu menu) {
        return false; // Return false if nothing is done
    }

    // Called when the user selects a contextual menu item
    @Override
    public boolean onActionItemClicked(ActionMode mode, MenuItem item) {
        switch (item.getItemId()) {
            case R.id.menu_share:
                shareCurrentItem();
                mode.finish(); // Action picked, so close the CAB
                return true;
            default:
                return false;
        }
    }

    // Called when the user exits the action mode
    @Override
    public void onDestroyActionMode(ActionMode mode) {
        mActionMode = null;
    }
    };

**----调用 startActionMode() 以便适时启用上下文操作模式，例如：响应对 View 的长按操作：**

    someView.setOnLongClickListener(new View.OnLongClickListener() {
    // Called when the user long-clicks on someView
    public boolean onLongClick(View view) {
        if (mActionMode != null) {
            return false;
        }

        // Start the CAB using the ActionMode.Callback defined above
        mActionMode = getActivity().startActionMode(mActionModeCallback);
        view.setSelected(true);
        return true;
    }
    });

**-在 ListView 或 GridView 中启用批处理上下文操作：**  
实现 AbsListView.MultiChoiceModeListener 接口，并使用 setMultiChoiceModeListener() 为视图组设置该接口。在侦听器的回调方法中，您既可以为上下文操作栏指定操作，也可以响应操作项目的点击事件，还可以处理从 ActionMode.Callback 接口继承的其他回调。  
使用 CHOICE_MODE_MULTIPLE_MODAL 参数调用 setChoiceMode()。

    ListView listView = getListView();
    listView.setChoiceMode(ListView.CHOICE_MODE_MULTIPLE_MODAL);
    listView.setMultiChoiceModeListener(new MultiChoiceModeListener() {

    @Override
    public void onItemCheckedStateChanged(ActionMode mode, int position,
                                          long id, boolean checked) {
        // Here you can do something when items are selected/de-selected,
        // such as update the title in the CAB
    }

    @Override
    public boolean onActionItemClicked(ActionMode mode, MenuItem item) {
        // Respond to clicks on the actions in the CAB
        switch (item.getItemId()) {
            case R.id.menu_delete:
                deleteSelectedItems();
                mode.finish(); // Action picked, so close the CAB
                return true;
            default:
                return false;
        }
    }

    @Override
    public boolean onCreateActionMode(ActionMode mode, Menu menu) {
        // Inflate the menu for the CAB
        MenuInflater inflater = mode.getMenuInflater();
        inflater.inflate(R.menu.context, menu);
        return true;
    }

    @Override
    public void onDestroyActionMode(ActionMode mode) {
        // Here you can make any necessary updates to the activity when
        // the CAB is removed. By default, selected items are deselected/unchecked.
    }

    @Override
    public boolean onPrepareActionMode(ActionMode mode, Menu menu) {
        // Here you can perform updates to the CAB due to
        // an invalidate() request
        return false;
    }
    });


## popupMenu ##
    public void showMenu(View v) {
    PopupMenu popup = new PopupMenu(this, v);

    // This activity implements OnMenuItemClickListener
    popup.setOnMenuItemClickListener(this);
    popup.inflate(R.menu.actions);
    popup.show();
    }

    @Override
    public boolean onMenuItemClick(MenuItem item) {
    switch (item.getItemId()) {
        case R.id.archive:
            archive(item);
            return true;
        case R.id.delete:
            delete(item);
            return true;
        default:
            return false;
    }
    }

## 使用可选中的菜单项 ##
使用 <item> 元素中的 android:checkable 属性为各个菜单项定义可选中的行为，或者使用 <group> 元素中的 android:checkableBehavior 属性为整个组定义可选中的行为。
##References
[https://github.com/codepath/android_guides/wiki/Menus-and-Popups](https://github.com/codepath/android_guides/wiki/Menus-and-Popups "refer")  
[http://developer.android.com/intl/zh-cn/guide/topics/ui/menus.html](http://developer.android.com/intl/zh-cn/guide/topics/ui/menus.html "refer")



