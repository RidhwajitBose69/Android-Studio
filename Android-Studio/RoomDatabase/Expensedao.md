//Interface for the DAO (Data Access Object) to manage the database operations
//for the Expense entity in the Room database.


package com.example.room;

import androidx.room.Dao;
import androidx.room.Delete;
import androidx.room.Insert;
import androidx.room.Query;
import androidx.room.Update;

import java.util.List;

@Dao

public interface Expensedao {
    @Query("select * from expense")
    List<Expense> getAllExpense();



    @Insert
    void addTx(Expense expense);

    @Update
    void updateTx(Expense expense);

    @Delete
    void deleteTx(Expense expense);
}
