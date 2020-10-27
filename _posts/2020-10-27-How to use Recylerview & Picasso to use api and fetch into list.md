---
layout: post
title: How to use Recylerview & Picasso to use api and fetch into list
published: true
---
# Android Snippets

## How to use Recylerview & Picasso to use api and fetch into list

##### Dependency/Gradle:

```gradle
//recyclerview
implementation 'androidx.recyclerview:recyclerview:1.1.0'

//Image Library
implementation 'com.squareup.picasso:picasso:2.71828' 

//for calling api
 implementation 'com.android.volley:volley:1.1.0'
```

#### Add Permission in Manifest.xml

For api call we will need internet

```xml
<uses-permission android:name="android.permission.INTERNET" />

```



### Steps:

#### Add recycler view in activitymain.xml

```xml
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerview"
        android:layout_width="409dp"
        android:layout_height="729dp"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

#### Create item view : itemfeed.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="#FFFFFF">

    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginStart="4dp"
        android:layout_marginLeft="4dp"
        android:layout_marginTop="4dp"
        android:layout_marginEnd="4dp"
        android:layout_marginRight="4dp"
        android:layout_marginBottom="4dp"
        android:background="#F4EFEF"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent">

        <ImageView
            android:id="@+id/profile"
            android:layout_width="60dp"
            android:layout_height="59dp"
            android:layout_marginStart="4dp"
            android:layout_marginLeft="4dp"
            android:layout_marginTop="16dp"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            tools:srcCompat="@tools:sample/avatars" />

        <TextView
            android:id="@+id/title"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginStart="24dp"
            android:layout_marginLeft="24dp"
            android:layout_marginTop="16dp"
            android:text="Title"
            android:textColor="#000000"
            android:textSize="24dp"
            app:layout_constraintStart_toEndOf="@+id/profile"
            app:layout_constraintTop_toTopOf="parent" />

        <TextView
            android:id="@+id/status"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginStart="2dp"
            android:layout_marginLeft="2dp"

            android:layout_marginTop="24dp"
            android:layout_marginEnd="2dp"
            android:layout_marginRight="2dp"
            android:paddingLeft="4dp"
            android:paddingRight="4dp"
            android:text="lorem  posem lorem posemlorem  posem lorem posemlorem  pose					posem"
            android:textSize="22dp"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/profile" />

        <ImageView
            android:id="@+id/image"
            android:layout_width="match_parent"
            android:layout_height="200dp"
            android:layout_marginTop="8dp"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintHorizontal_bias="0.0"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/status"
            tools:srcCompat="@tools:sample/backgrounds/scenic" />

        <TextView
            android:id="@+id/time"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginStart="28dp"
            android:layout_marginLeft="28dp"
            android:layout_marginTop="10dp"
            android:text="TextView"
            app:layout_constraintStart_toEndOf="@+id/profile"
            app:layout_constraintTop_toBottomOf="@+id/title" />
    </androidx.constraintlayout.widget.ConstraintLayout>
</androidx.constraintlayout.widget.ConstraintLayout>
```

#### Based On data Create Model class :

##### Here we have used api ,to parse so according to data it is the modelclass.java

```java
package com.saurabhjadhav.quantsapp_task;

public class model_class {

    private  String title;
    private String profilepic;
    private String image;
    private  String status;
    private  String  time;

    public model_class(String title, String profilepic, String image, String status, String time) {
        this.title = title;
        this.profilepic = profilepic;
        this.image = image;
        this.status = status;
        this.time = time;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getProfilepic() {
        return profilepic;
    }

    public void setProfilepic(String profilepic) {
        this.profilepic = profilepic;
    }

    public String getImage() {
        return image;
    }

    public void setImage(String image) {
        this.image = image;
    }

    public String getStatus() {
        return status;
    }

    public void setStatus(String status) {
        this.status = status;
    }

    public String getTime() {
        return time;
    }

    public void setTime(String time) {
        this.time = time;
    }
}

```

#### Now Create Adapter class for recycler view:

##### Recycleradapter.java

```java
package com.saurabhjadhav.quantsapp_task;

import android.content.Context;
import android.text.format.DateUtils;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ImageView;
import android.widget.TextView;

import androidx.annotation.NonNull;
import androidx.recyclerview.widget.RecyclerView;

import com.squareup.picasso.Picasso;

import java.util.ArrayList;

public class recycler_adapter extends RecyclerView.Adapter<recycler_adapter.recyclerviewHolder> {

    private Context mcontext;
    private ArrayList<model_class>  mmodelClassArrayList;

    public  recycler_adapter(Context context,ArrayList<model_class> modelClassArrayList)
    {
        mcontext=context;
        mmodelClassArrayList=modelClassArrayList;
    }

    @NonNull
    @Override
    public recyclerviewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {

        View v= LayoutInflater.from(mcontext).inflate(R.layout.item_feed,parent,false);
        return new recyclerviewHolder(v);


    }

    @Override
    public void onBindViewHolder(@NonNull recyclerviewHolder holder, int position) {
            model_class currentitem=mmodelClassArrayList.get(position);
            String title1=currentitem.getTitle();
            String image1=currentitem.getImage();
            String profile1=currentitem.getProfilepic();
            String status1=currentitem.getStatus();

            holder.status.setText(status1);
            holder.title.setText(title1);

        CharSequence timeAgo = DateUtils.getRelativeTimeSpanString(
                Long.parseLong(currentitem.getTime()),
                System.currentTimeMillis(), DateUtils.SECOND_IN_MILLIS);
        holder.time2.setText(timeAgo);

        Picasso.get().load(profile1).into(holder.profile2);
        Picasso.get().load(image1).into(holder.Image2);

    }

    @Override
    public int getItemCount() {
        return mmodelClassArrayList.size();
    }


    public class  recyclerviewHolder extends  RecyclerView.ViewHolder{

        public ImageView profile2,Image2;
        public TextView title,status,time2;

        public recyclerviewHolder(@NonNull View itemView) {
            super(itemView);
            profile2=itemView.findViewById(R.id.profile);
            Image2=itemView.findViewById(R.id.image);
            title=itemView.findViewById(R.id.title);
            status=itemView.findViewById(R.id.status);
            time2=itemView.findViewById(R.id.time);
        }
    }
}

```

#### Now Open MainActivity.java

Declare variables and in On create Bind the adapter and recycler view:

```java
public class MainActivity extends AppCompatActivity {

    private RecyclerView mrecyclerView;
    private  recycler_adapter mrecycler_adapter;
    private ArrayList<model_class> model_classArrayList;
    private RequestQueue mrequestQueue;



    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

            mrecyclerView=findViewById(R.id.recyclerview);
            mrecyclerView.setLayoutManager(new LinearLayoutManager(this));
            model_classArrayList=new ArrayList<>();
            mrequestQueue= Volley.newRequestQueue(this);
            parseJSON();//this is for parsing we will create it



    }

   
}
```

Now define parseJSON where we will use Volley:

```java
private  void parseJSON()
    {
        String url="https://api.androidhive.info/feed/feed.json";

        JsonObjectRequest request=new JsonObjectRequest(Request.Method.GET, url, null,
                new Response.Listener<JSONObject>() {
                    @Override
                    public void onResponse(JSONObject response) {
                        try {
                            JSONArray jsonArray=response.getJSONArray("feed");

                            for (int i=0;i<jsonArray.length();i++)
                            {
                                JSONObject feed=jsonArray.getJSONObject(i);


                                String title1=feed.getString("name");
                                String image1=feed.getString("image");
                                String profile1=feed.getString("profilePic");
                                String status1=feed.getString("status");
                                String time=feed.getString("timeStamp");
                                model_classArrayList.add(new model_class(title1,profile1,image1,status1,time));
                            }
                            mrecycler_adapter=new recycler_adapter(MainActivity.this
                            ,model_classArrayList);
                            mrecyclerView.setAdapter(mrecycler_adapter);

                        } catch (JSONException e) {
                            e.printStackTrace();
                        }

                    }
                }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                error.printStackTrace();
            }
        });
        mrequestQueue.add(request);


    }
```

##### Full Mainactivity.java

```java
package com.saurabhjadhav.quantsapp_task;

import androidx.appcompat.app.AppCompatActivity;
import androidx.recyclerview.widget.LinearLayoutManager;
import androidx.recyclerview.widget.RecyclerView;

import android.os.Bundle;

import com.android.volley.Request;
import com.android.volley.RequestQueue;
import com.android.volley.Response;
import com.android.volley.VolleyError;
import com.android.volley.toolbox.JsonObjectRequest;
import com.android.volley.toolbox.Volley;

import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

import java.util.ArrayList;

public class MainActivity extends AppCompatActivity {

    private RecyclerView mrecyclerView;
    private  recycler_adapter mrecycler_adapter;
    private ArrayList<model_class> model_classArrayList;
    private RequestQueue mrequestQueue;



    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

            mrecyclerView=findViewById(R.id.recyclerview);
            mrecyclerView.setLayoutManager(new LinearLayoutManager(this));
            model_classArrayList=new ArrayList<>();
            mrequestQueue= Volley.newRequestQueue(this);
            parseJSON();



    }

    private  void parseJSON()
    {
        String url="https://api.androidhive.info/feed/feed.json";

        JsonObjectRequest request=new JsonObjectRequest(Request.Method.GET, url, null,
                new Response.Listener<JSONObject>() {
                    @Override
                    public void onResponse(JSONObject response) {
                        try {
                            JSONArray jsonArray=response.getJSONArray("feed");

                            for (int i=0;i<jsonArray.length();i++)
                            {
                                JSONObject feed=jsonArray.getJSONObject(i);


                                String title1=feed.getString("name");
                                String image1=feed.getString("image");
                                String profile1=feed.getString("profilePic");
                                String status1=feed.getString("status");
                                String time=feed.getString("timeStamp");
                                model_classArrayList.add(new model_class(title1,profile1,image1,status1,time));
                            }
                            mrecycler_adapter=new recycler_adapter(MainActivity.this
                            ,model_classArrayList);
                            mrecyclerView.setAdapter(mrecycler_adapter);

                        } catch (JSONException e) {
                            e.printStackTrace();
                        }

                    }
                }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                error.printStackTrace();
            }
        });
        mrequestQueue.add(request);


    }
}
```





#### Hope All of you Understands it and Got code to use it asap in your project!
