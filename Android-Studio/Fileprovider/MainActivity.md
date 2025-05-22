package com.example.filep;

import android.app.Activity;
import android.content.Intent;
import android.graphics.Bitmap;
import android.graphics.ImageDecoder;
import android.graphics.drawable.BitmapDrawable;
import android.net.Uri;
import android.os.Build;
import android.os.Bundle;
import android.provider.MediaStore;
import android.text.TextUtils;
import android.view.View;
import android.widget.ImageView;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.activity.result.ActivityResult;
import androidx.activity.result.ActivityResultCallback;
import androidx.activity.result.ActivityResultLauncher;
import androidx.activity.result.contract.ActivityResultContracts;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.content.FileProvider;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;

import androidx.core.view.WindowInsetsCompat;

import com.example.filep.databinding.ActivityMainBinding;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;


public class MainActivity extends AppCompatActivity {
    ImageView imageView;
    private ActivityMainBinding binding;
private Uri imageUri=null;
private String textToShare="";
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        binding=ActivityMainBinding.inflate(getLayoutInflater());
        setContentView(binding.getRoot());
        binding.imageView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                pickImage();
            }
        });
        binding.sharetextbtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
             textToShare=binding.textEt.getText().toString().trim();
             if(TextUtils.isEmpty(textToShare)){
                 Toast.makeText(MainActivity.this, "Enter Text", Toast.LENGTH_SHORT).show();
             }
             else {
                 shareText();
             }
            }
        });
        binding.shareimagebtn.setOnClickListener(new View.OnClickListener() {//  alternative for findviewbyid
            @Override
            public void onClick(View v) {
                if(imageUri==null){
                    Toast.makeText(MainActivity.this, "Pick image...", Toast.LENGTH_SHORT).show();
                }
                else {
                    shareImage();
                }

            }
        });
        binding.sharebothbtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                textToShare=binding.textEt.getText().toString().trim();// textEt is id
                if(TextUtils.isEmpty(textToShare)){
                    Toast.makeText(MainActivity.this, "Enter Text", Toast.LENGTH_SHORT).show();
                }
                else{
                  shareBoth();
                }
            }
        });
    }

    private void pickImage() {
        Intent intent=new Intent(Intent.ACTION_PICK);
        intent.setType("image/*");// MIME multipurpose internet mail extension way to indicate type of file set to handle any type of files related to image
        galleryActivityResultLauncher.launch(intent);// go to gallery
    }
    private ActivityResultLauncher<Intent> galleryActivityResultLauncher=registerForActivityResult(// allows launch activities
            new ActivityResultContracts.StartActivityForResult(),
            new ActivityResultCallback<ActivityResult>() {// retriew result here
                @Override
                public void onActivityResult(ActivityResult result) {
                   if(result.getResultCode()== Activity.RESULT_OK){
                       Toast.makeText(MainActivity.this, "Image picked from gallery", Toast.LENGTH_SHORT).show();
                       Intent data=result.getData();
                       imageUri=data.getData();
                       binding.imageView.setImageURI(imageUri);
                   }
                   else {
                       Toast.makeText(MainActivity.this, "Cancelled", Toast.LENGTH_SHORT).show();
                   }
                }
            }


    );
    private  void shareText(){
        Intent shareIntent = new Intent(Intent.ACTION_SEND);
        shareIntent.setType("text/plain");
        shareIntent.putExtra(Intent.EXTRA_SUBJECT,"Subject Here");
        shareIntent.putExtra(Intent.EXTRA_TEXT,textToShare);
        startActivity(Intent.createChooser(shareIntent,"Share Via"));
    }

    private void shareImage(){
        Uri contentUri = getContentUri();
        Intent shareIntent = new Intent(Intent.ACTION_SEND);
        shareIntent.setType("text/png");
        shareIntent.putExtra(Intent.EXTRA_SUBJECT,"Subject Here");
        shareIntent.putExtra(Intent.EXTRA_STREAM,contentUri);
        shareIntent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);// temporary access read write
        startActivity(Intent.createChooser(shareIntent,"Share Via"));
    }
    private void shareBoth(){
        Uri contentUri = getContentUri();
        Intent shareIntent = new Intent(Intent.ACTION_SEND);
        shareIntent.setType("text/png");
        shareIntent.putExtra(Intent.EXTRA_SUBJECT,"Subject Here");
        shareIntent.putExtra(Intent.EXTRA_STREAM,textToShare);
        shareIntent.putExtra(Intent.EXTRA_STREAM,contentUri);
        shareIntent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
        startActivity(Intent.createChooser(shareIntent,"Share Via"));

    }
    private  Uri getContentUri(){// purpose take image in bitmap and store it in new directory and cache  and share it through uri
        Bitmap bitmap = null;// uri is safer versioon to access because it does not provide actual path
        try {
            if(Build.VERSION.SDK_INT>=Build.VERSION_CODES.P){// greater than 9 pie
                ImageDecoder.Source source = ImageDecoder.createSource(getContentResolver(),imageUri);// image decode in latest versio
                bitmap=ImageDecoder.decodeBitmap(source);
            }
            else {
               bitmap= MediaStore.Images.Media.getBitmap(getContentResolver(),imageUri);
            }
        } catch (IOException e) {
            Toast.makeText(this, " "+e.getMessage(), Toast.LENGTH_SHORT).show();
        }

        BitmapDrawable bitmapDrawable=(BitmapDrawable) binding.imageView.getDrawable();
        File imageFolder = new File(getCacheDir(),"images");// temp files name of dire
        Uri contentUri=null;// initialization
        try {
           imageFolder.mkdirs();
           File file =new File(imageFolder,"shared_image.png");
            FileOutputStream stream = new FileOutputStream(file);// for writing binary data
            bitmap.compress(Bitmap.CompressFormat.PNG,50,stream);
            stream.flush();//Relese the resources
            stream.close();// file_path tells fileprovider what type of directories can be shared checks weather it matches path  mentioned in file path
            contentUri= FileProvider.getUriForFile(this,"com.example.filep",file);// checks if it is allowed directoy acc to file.xml
        }
        catch (Exception e){
            Toast.makeText(this, " "+e.getMessage(), Toast.LENGTH_SHORT).show();
        }
        return contentUri;
    }
}