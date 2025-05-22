package com.example.retrofit;

import java.util.List;

import retrofit2.Call;
import retrofit2.http.GET;

public interface myapi {
    @GET("posts")// specifies http method
     Call<List<Model>> getmodels();// call retrofit request getmodels() method without waiting(asyncronised)

}
