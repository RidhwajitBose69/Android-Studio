package com.example.service;

import android.app.Service;
import android.content.Intent;
import android.media.MediaPlayer;
import android.os.IBinder;
import android.provider.Settings;
import androidx.annotation.Nullable;

public class MusicService extends Service {
    MediaPlayer mediaPlayer;

    @Nullable
    @Override// activity bind to service through ipc inter process communication establish connection between activity and service
    public IBinder onBind(Intent intent) {// between activity and service interprocess communication ipc activity and service connected interprocess communication
        return null;// no ipc
    }// makes independent another component

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        mediaPlayer = MediaPlayer.create(this, Settings.System.DEFAULT_RINGTONE_URI);// we are retrieving from users settings
        if (mediaPlayer != null) { // Check if MediaPlayer was created successfully
            mediaPlayer.setLooping(true);
            mediaPlayer.start();
        }
        return START_NOT_STICKY;// don't restart automatically
    }

    @Override
    public void onDestroy() {
        if (mediaPlayer != null) {
            mediaPlayer.stop();
            mediaPlayer.release(); // Release the resources
            mediaPlayer = null; // Set to null
        }
        super.onDestroy();
    }
}