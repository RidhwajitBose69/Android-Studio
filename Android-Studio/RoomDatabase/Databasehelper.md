//This is a Room database helper class for managing the database of expenses.
// It extends RoomDatabase and provides a singleton instance of the database.



package com.example.room;

import android.content.Context;

import androidx.room.Database;
import androidx.room.Room;
import androidx.room.RoomDatabase;
@Database(entities = Expense.class, exportSchema = false, version = 1)
abstract class Dtabasehelper extends RoomDatabase {
    private static final String DB_NAME = "expensedb";// file name
    private static Dtabasehelper instance;

    public static synchronized Dtabasehelper getDB(Context context) {
        if (instance == null) {
            instance = Room.databaseBuilder(context.getApplicationContext(), Dtabasehelper.class, DB_NAME)
                    .fallbackToDestructiveMigration()// create new database sometimes where it cannot perform migration
                    .allowMainThreadQueries()// Brings UI to main thread
                    .build();
        }
        return instance;
    }

    public abstract Expensedao expensedao();
}

