 package com.example.room;

import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

import java.util.ArrayList;

 public class MainActivity extends AppCompatActivity {
EditText editText,editText2;
Button btnNext;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            editText=findViewById(R.id.editText);
            editText2=findViewById(R.id.editText2);
            btnNext=findViewById(R.id.btnNext);
            Dtabasehelper databaseHelper = Dtabasehelper.getDB(this);
            btnNext.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    String title =editText.getText().toString();
                    String amount=editText2.getText().toString();
                    databaseHelper.expensedao().addTx(
                            new Expense(title,amount) // id is autogennerated user give only title and amount thus constructor of 2 p is used
                    );
                   ArrayList<Expense>arrExpenses =(ArrayList<Expense>) databaseHelper.expensedao().getAllExpense();
                   for(int i=0;i<arrExpenses.size();i++){
                       Log.d("Data","Title:"+arrExpenses.get(i).getTitle()+"Amount: "+arrExpenses.get(i).getAmount()+"ID: "+arrExpenses.get(i).getId());
                   }
                }
            });


            return insets;
        });
    }
}