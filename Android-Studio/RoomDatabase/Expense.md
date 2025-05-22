package com.example.room;

import androidx.room.ColumnInfo;
import androidx.room.Entity;
import androidx.room.Ignore;
import androidx.room.PrimaryKey;

@Entity(tableName = "expense")// Entity for table
public class Expense {
    @PrimaryKey(autoGenerate = true)
    private int id;
    @ColumnInfo(name="title")
    private String title;
    @ColumnInfo(name="amount")
    private String amount;
    Expense(int id,String title,String amount) {
        this.id = id;
        this.amount = amount;
        this.title = title;
    }
    @Ignore// Skip this
        Expense(String title,String amount){
            this.title=title;
            this.amount=amount;
        }

    public int getId() {
        return id;
    }

    public String getAmount() {
        return amount;
    }

    public String getTitle() {
        return title;
    }

    public void setAmount(String amount) {
        this.amount = amount;
    }

    public void setId(int id) {
        this.id = id;
    }

    public void setTitle(String title) {
        this.title = title;
    }
}
