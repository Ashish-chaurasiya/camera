# camera
android project to capture image and save in directory in high quality

---
# 1st step
## create XML file mainActivity.xml
```xml
    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Camera"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/button"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:onClick="CaptureImage"
        android:text="Open Camera"
        app:layout_constraintBottom_toTopOf="@+id/button1"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent" />

    <Button
        android:id="@+id/button1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="20dp"
        android:onClick="diplayImage"
        android:text="Show Catured Image"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent" />
```
***
# 2nd step
## java code MainActivity.java
```JavaScript
package com.ash.camera;

import androidx.appcompat.app.AppCompatActivity;
import androidx.core.content.FileProvider;

import android.content.Intent;
import android.graphics.Bitmap;
import android.net.Uri;
import android.os.Bundle;
import android.os.Environment;
import android.provider.ContactsContract;
import android.provider.ContactsContract.RawContacts.Data;
import android.provider.MediaStore;
import android.view.View;
import android.widget.Button;
import android.widget.ImageView;

import java.io.File;
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.BitSet;
import java.util.Date;

public class MainActivity extends AppCompatActivity {
    ImageView imageView;
    Button button;
    String currentImagePath = null;
    private static  final int IMAGE_REQUEST =1;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        button = (Button) findViewById(R.id.button);


    }


    public void CaptureImage(View view) {
        Intent cameraIntent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
        if(cameraIntent.resolveActivity(getPackageManager())!=null)
        {
            File imageFile = null;
            try {
                imageFile=getImageFile();

            }
            catch (IOException e){
                e.printStackTrace();

            }
            if(imageFile!=null)
            {
                Uri imageUri = FileProvider.getUriForFile(this,"com.example.android.fileprovider",imageFile) ;
                cameraIntent.putExtra(MediaStore.EXTRA_OUTPUT,imageUri);
                startActivityForResult(cameraIntent,IMAGE_REQUEST);


            }
        }
    }

    public void diplayImage(View view) {
        Intent intent =new Intent(this,Displayimage.class);
        intent.putExtra("image_path",currentImagePath);
        startActivity(intent);
    }
    private File getImageFile() throws IOException
    {
        String timeStamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
        String imageName = "jpg"+timeStamp+"_";
        File storageDir = getExternalFilesDir(Environment.DIRECTORY_PICTURES);
        File imageFile = File.createTempFile(imageName,".jpg",storageDir);
        currentImagePath = imageFile.getAbsolutePath();
        return imageFile;




    }
}
```
***
# 3rd step 
## Add user permition in mainifest
```xml
<uses-permission
        android:name="android.permission.WRITE_EXTERNAL_STORAGE"
        android:maxSdkVersion="18" />
```
***
# 4th step 
## create java and Xml activity
## File name = Displayimage.java
## File name = activity_displayimage.xml
***
# 5th step
## Displayimage.java
```JavaScript
package com.ash.camera;

import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.ImageView;

public class Displayimage extends AppCompatActivity {
    ImageView imageView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_displayimage);
        imageView = findViewById(R.id.mimageView);

        Bitmap bitmap = BitmapFactory.decodeFile(getIntent().getStringExtra("image_path"));
        imageView.setImageBitmap((bitmap));


    }

    }

```
# 6th step
## activity_displayimage.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".Displayimage">

    <ImageView
        android:id="@+id/mimageView"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        tools:ignore="MissingConstraints" />


</androidx.constraintlayout.widget.ConstraintLayout>
```
# 7th step
## Create "xml" folder in "res" folder and create one file_path.xml
## file_path.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<paths xmlns:android="http://schemas.android.com/apk/res/android">
    <external-path
        name="my_image"
        path="Android/data/com.ash.camera/files/Pictures"/>

</paths>
```
![Main screen](https://photos.app.goo.gl/mMmbF7CciuubwZv6A)
<img src ="https://photos.app.goo.gl/mMmbF7CciuubwZv6A">

## Click on Camera button
![1](https://user-images.githubusercontent.com/74813584/99874858-fbc4a700-2c10-11eb-8bdf-87eb7d93271b.jpg)
## Capter images
![2](https://user-images.githubusercontent.com/74813584/99874925-84dbde00-2c11-11eb-98e3-a22a742754b7.jpg)
## Click on 2nd button to view images
![3](https://user-images.githubusercontent.com/74813584/99874939-9a510800-2c11-11eb-80ef-f475213960bd.jpg)


