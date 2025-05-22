package com.example.recycler;

import android.content.Context;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ImageView;
import android.widget.TextView;

import androidx.annotation.NonNull;
import androidx.recyclerview.widget.RecyclerView;

import java.util.ArrayList;

public class RecyclerContactAdapter extends RecyclerView.Adapter<RecyclerContactAdapter.ViewHolder> {//here .ViewHolder is there by  this we can access ViewHolder and this is nested and closely combined
    Context context;
    ArrayList<ContactModel> arrContacts;
    RecyclerContactAdapter(Context context, ArrayList<ContactModel>arrContacts){

        this.context=context;
        this.arrContacts=arrContacts;
    }
    @NonNull // return type should not be null
    @Override
    public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {// its like factory for viewholder objects
       View itemview= LayoutInflater.from(context).inflate(R.layout.contact_row,parent,false);// parse xml and repeatedly view layout layout inside layout
       ViewHolder viewHolder= new ViewHolder(itemview);
        return viewHolder;
    }

    @Override
    public void onBindViewHolder(@NonNull ViewHolder holder, int position) {// id views are in this holder class
        holder.imageContact.setImageResource(arrContacts.get(position).img);
        holder.txtName.setText(arrContacts.get(position).name);// set it in position
        holder.txtNumber.setText(arrContacts.get(position).number);

    }

    @Override
    public int getItemCount() {
        return arrContacts.size();
    }

    public static class ViewHolder extends RecyclerView.ViewHolder{
        TextView txtName,txtNumber;
        ImageView imageContact;

        public ViewHolder(@NonNull View itemView) {// ViewHolder class to extract textview all these in this class because we cant use findviewbyid here
            super(itemView);// invokes this from upper class passes layout inflator here invokes constructor from library
            txtName = itemView.findViewById(R.id.textView);// now we can use it
            txtNumber=itemView.findViewById(R.id.textView2);// In itemView View is there
            imageContact=itemView.findViewById(R.id.imageView);
        }
    }
}
