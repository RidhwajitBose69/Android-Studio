package com.example.deeplinking;

import android.net.Uri;
import android.os.Bundle;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

import java.util.List;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);

            Uri uri = getIntent().getData();// a string rthat identifies resource
            if (uri != null) {
                List<String> params = uri.getPathSegments();// it divides uri into paths
                if (params != null && params.size() >= 2) {// atleast 2 segments
                    String id = params.get(1); // Get the second segment (12345)
                    Toast.makeText(this, "ID: " + id, Toast.LENGTH_SHORT).show();
                } else {
                    Toast.makeText(this, "Invalid URL format", Toast.LENGTH_SHORT).show();
                }
            } else {
                Toast.makeText(this, "No URI found", Toast.LENGTH_SHORT).show();
            }
            return insets;
        });
    }
}