# NotePad
## **1.项目结构**

![image](https://github.com/Lauyoun/schoolRepository/blob/master/screenShot/ProjectStructure.png?raw=true)

### **主要类**

1. NotesList类 —— 应用程序的入口，笔记本的首页面会显示笔记的列表
2. NoteEditor类 —— 编辑笔记内容的Activity
3. TitleEditor类 —— 编辑笔记标题的Activity
4. NotePadProvider类 —— 这是笔记本应用的ContentProvider，也是整个应用的关键所在
5. NotesLiveFolder ContentProvider的LiveFolder（实时文件夹），这个功能在Android API 14后被废弃，不再支持。因此代码中所有涉及LiveFolder的内容将不再阐述

### **布局文件**

![image](https://github.com/Lauyoun/schoolRepository/blob/master/screenShot/Layout.png?raw=true)

### **菜单文件**

![image](https://github.com/Lauyoun/schoolRepository/blob/master/screenShot/menu.png?raw=true)

## **2.添加时间戳**

2.1应该在NotePad主页面的每个笔记后面都添加时间显示，应该在`notelist_item.xml`布局文件中添加`TextView`

添加的代码应为：

```
<TextView
    android:id="@android:id/text2"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:gravity="center_vertical"
    android:paddingLeft="5dp"
    android:singleLine="true" />
```

修改后的`notelist_item.xml`

```
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent" android:layout_height="match_parent">
<TextView
    android:id="@android:id/text1"
    android:layout_width="match_parent"
    android:layout_height="?android:attr/listPreferredItemHeight"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:gravity="center_vertical"
    android:paddingLeft="5dip"
    android:singleLine="true"
/>
    <TextView
        android:id="@android:id/text2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:gravity="center_vertical"
        android:paddingLeft="5dp"
        android:singleLine="true" />

</FrameLayout>
```

2.2NoteList类对应的`PROJECTION`中添加`COLUMN_NAME_MODIFICATION_DATE`

![image](https://github.com/Lauyoun/schoolRepository/blob/master/screenShot/PROJECTION.png?raw=true)

修改后

```
private static final String[] PROJECTION = new String[] {
        NotePad.Notes._ID, // 0
        NotePad.Notes.COLUMN_NAME_TITLE,//1
        NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE,
};
```

2.3修改适配器内容，在NoteList类增加dataColumns中装配到ListView的内容

![image](https://github.com/Lauyoun/schoolRepository/blob/master/screenShot/PROJECTION.png?raw=true)

修改后

```
// The names of the cursor columns to display in the view, initialized to the title column
String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE,
        NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE } ;

// The view IDs that will display the cursor columns, initialized to the TextView in
// noteslist_item.xml
int[] viewIDs = { android.R.id.text1,android.R.id.text2 };
```

2.4对时间进行格式化

```
// Sets up a map to contain values to be updated in the provider.
ContentValues values = new ContentValues();
Long now = Long.valueOf(System.currentTimeMillis());
SimpleDateFormat sf = new SimpleDateFormat("yy/MM/dd HH:mm");
Date d = new Date(now);
String format = sf.format(d);
values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, format);
```

2.5添加时间戳后运行效果

![image](https://user-images.githubusercontent.com/63631274/118487178-089ffe80-b74d-11eb-9e12-e80a636eaa4d.png)



