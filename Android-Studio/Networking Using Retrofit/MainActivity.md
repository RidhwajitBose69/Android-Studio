package com.example.retrofit;// HTTP requests

import android.os.Bundle;
import android.widget.TextView;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

import java.util.List;

import retrofit2.Call;
import retrofit2.Callback;
import retrofit2.Response;
import retrofit2.Retrofit;
import retrofit2.converter.gson.GsonConverterFactory;

public class MainActivity extends AppCompatActivity {
    TextView textView;
    String url="https://jsonplaceholder.typicode.com/";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            textView=findViewById(R.id.textView);
            textView.setText("");
            Retrofit retrofit = new Retrofit.Builder()
                    .baseUrl(url)
                    .addConverterFactory(GsonConverterFactory.create())// convert raw data(JSON) to java objects javascript object notation
                    .build();
            myapi api=retrofit.create(myapi.class);// implement myapi interface @GET here and store  in api dynamically
            Call<List<Model>>call=api.getmodels();// fetch from @GET and store in call of type List<model>
            call.enqueue(new Callback<List<Model>>() {// enqueue for network request in bthread callback is object of anony class of callback interface to handle responses
                @Override// Call is used for single HTTP requset
                public void onResponse(Call<List<Model>> call, Response<List<Model>> response) {
                    List<Model> data=response.body();//after successful fetch json or xml into java objects and store in data of type List<model>
                    for (int i=0;i< data.size();i++){
                        textView.append(" SL No: "+data.get(i).getId()+" \nTitle: "+data.get(i).getTitle()+"\n\n");//append in textview
                    }
                }

                @Override
                public void onFailure(Call<List<Model>> call, Throwable t) {

                }
            });
            return insets;
        });
    }
}