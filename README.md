# ReactPrograms
Android program 
contact list ....
package com.example.mcaapplication

import android.database.Cursor
import android.graphics.Color
import android.os.Bundle
import android.provider.ContactsContract
import android.widget.SimpleCursorAdapter
import androidx.appcompat.app.AppCompatActivity
import com.example.mcaapplication.databinding.ActivityContactListBinding
import com.example.mcaapplication.databinding.ActivityForgotPasswordBinding

class ContactListActivity : AppCompatActivity() {

    private lateinit var cBinding : ActivityContactListBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        cBinding = ActivityContactListBinding.inflate(layoutInflater)
        setContentView(cBinding.root)


        readContact()

    }


    private fun readContact()
    {
        var cursor : Cursor = contentResolver.query(ContactsContract.CommonDataKinds.
        Phone.CONTENT_URI,
            null,null,null,null)!!

        var contactNo = arrayOf(ContactsContract.CommonDataKinds.Phone.NUMBER)

        var txtView = intArrayOf(android.R.id.text1)

        var simpleAdapter : SimpleCursorAdapter =
            SimpleCursorAdapter(this,android.R.layout.simple_list_item_1,
                cursor,contactNo,txtView)

        cBinding.lvContact.adapter = simpleAdapter

    }



}
---_------------------------------------
video.kt 
package com.example.mcaapplication

import android.media.MediaPlayer
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.view.View
import android.widget.MediaController
import com.example.mcaapplication.databinding.ActivityVideoBinding

class VideoActivity : AppCompatActivity() {

    private lateinit var binding : ActivityVideoBinding


    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityVideoBinding.inflate(layoutInflater)
        setContentView(binding.root)


        binding.videoViewDemo.setVideoPath("android.resource://"+packageName+"/"+R.raw.demo)

        var media : MediaController = MediaController(this)

        media.setAnchorView(binding.videoViewDemo)

       binding.videoViewDemo.setMediaController(media)



        binding.videoViewDemo.setOnPreparedListener(MediaPlayer.OnPreparedListener {
            mp -> mp.isLooping = true
        })
    }

}
-----------------------------------------------&-
threaddemo.kt
package com.example.mcaapplication

import android.media.MediaPlayer
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.view.View
import android.widget.MediaController
import com.example.mcaapplication.databinding.ActivityVideoBinding

class VideoActivity : AppCompatActivity() {

    private lateinit var binding : ActivityVideoBinding


    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityVideoBinding.inflate(layoutInflater)
        setContentView(binding.root)


        binding.videoViewDemo.setVideoPath("android.resource://"+packageName+"/"+R.raw.demo)

        var media : MediaController = MediaController(this)

        media.setAnchorView(binding.videoViewDemo)

       binding.videoViewDemo.setMediaController(media)



        binding.videoViewDemo.setOnPreparedListener(MediaPlayer.OnPreparedListener {
            mp -> mp.isLooping = true
        })
    }

}
----------------------------------------------
home.kt
package com.example.mcaapplication

import android.content.Intent
import android.content.SharedPreferences
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.util.Log
import android.view.View
import com.example.mcaapplication.databinding.ActivityHomeBinding

class HomeActivity : AppCompatActivity() {

    private lateinit var binding : ActivityHomeBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityHomeBinding.inflate(layoutInflater)
        setContentView(binding.root)

//        binding.txtEmail.text = intent.getStringExtra("EMAIL")
//        binding.txtPass.text = intent.getStringExtra("PASS")

        binding.txtEmail.text = intent.getStringExtra("DATA")

        var intentData  = intent.getStringExtra("DATA")

        Log.d("TAG>>>>>>>>>>>>>>>>>>", intent.getStringExtra("DATA").toString())

        if(intentData == "Android")
        {
            binding.btnIOS.visibility = View.INVISIBLE
            binding.btnFlutter.visibility = View.INVISIBLE
            binding.btnAngular.visibility = View.INVISIBLE
            binding.btnCS.visibility = View.INVISIBLE
            binding.btnDC.visibility = View.INVISIBLE
        }
        else if(intentData == "ios")
        {
            binding.btnAndroid.visibility = View.INVISIBLE
            binding.btnFlutter.visibility = View.INVISIBLE
            binding.btnAngular.visibility = View.INVISIBLE
            binding.btnCS.visibility = View.INVISIBLE
            binding.btnDC.visibility = View.INVISIBLE
        }

        binding.txtLogout.setOnClickListener {
            var sharedPref : SharedPreferences = this.getSharedPreferences("MCAPref", MODE_PRIVATE)

            var editPREF : SharedPreferences.Editor = sharedPref.edit()

            editPREF.clear()

            editPREF.commit()

            startActivity(Intent(this,LoginActivity::class.java))
            finishAffinity() // All screen are Close
        }

    }
}
----------+++---------------------6--66
callavtivity.kt
package com.example.mcaapplication

import android.annotation.SuppressLint
import android.content.Context
import android.content.DialogInterface
import android.content.Intent
import android.net.Uri
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.view.View
import android.widget.AdapterView
import android.widget.AdapterView.OnItemClickListener
import android.widget.ArrayAdapter
import android.widget.Toast
import androidx.appcompat.app.AlertDialog
import androidx.core.view.get
import com.example.mcaapplication.databinding.ActivityCallBinding

class CallActivity : AppCompatActivity() {

    private lateinit var cBinding : ActivityCallBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        cBinding = ActivityCallBinding.inflate(layoutInflater)
        setContentView(cBinding.root)

        var listAdapter = ArrayAdapter(this,android.R.layout.simple_list_item_1,resources.getStringArray(R.array.subject))

        cBinding.listView.adapter = listAdapter

        cBinding.listView.onItemClickListener = object : OnItemClickListener{
            @SuppressLint("SuspiciousIndentation")
            override fun onItemClick(
                parent: AdapterView<*>?,
                view: View?,
                position: Int,
                id: Long
            ) {


//
             var itemData = cBinding.listView.getItemAtPosition(position) as String

                dialogShow(itemData)
//
//                var itemID = cBinding.listView.getItemIdAtPosition(position)
//
//                Toast.makeText(this@CallActivity,itemID.toString(),Toast.LENGTH_SHORT).show()
//
//                startActivity(Intent(this@CallActivity,
//                    HomeActivity::class.java).putExtra("DATA",itemData))

            }
        }
    }

    fun dialogShow(name:String)
    {
        var builder = AlertDialog.Builder(this)
        builder.setTitle(name)
        builder.setMessage("Are you sure you want to Exit from this App?")
        builder.setIcon(R.drawable.email)

        builder.setPositiveButton("Yes",DialogInterface.OnClickListener { dialog, which ->
            Toast.makeText(this,"This is Positive Button",Toast.LENGTH_LONG).show()
        })

        builder.setNegativeButton("NO",DialogInterface.OnClickListener { dialog, which ->
            Toast.makeText(this,"This is Cancellation Button",Toast.LENGTH_LONG).show()
        })


        var alertShow : AlertDialog = builder.create()
        alertShow.setCancelable(false)
        alertShow.show()

    }


}
---------------------------------------------------
getdataactivity.kt
package com.example.mcaapplication

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.util.Log
import android.widget.Toast
import com.example.mcaapplication.databinding.ActivityGetDataBinding
import com.google.firebase.database.DataSnapshot
import com.google.firebase.database.DatabaseError
import com.google.firebase.database.DatabaseReference
import com.google.firebase.database.FirebaseDatabase
import com.google.firebase.database.ValueEventListener

class GetDataActivity : AppCompatActivity() {

    private lateinit var binding : ActivityGetDataBinding
    private lateinit var dataBaseReference : DatabaseReference


    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityGetDataBinding.inflate(layoutInflater)
        setContentView(binding.root)

        binding.btnRead.setOnClickListener {
            readData(binding.edtMobileNo.text.toString())
        }

    }

   private fun readData(mobileNo : String)
    {
        dataBaseReference = FirebaseDatabase.getInstance().getReference("Emp")
        dataBaseReference.child(mobileNo).get().addOnSuccessListener {
            if(it.exists())
            {
                binding.edtFName.setText(it.child("fname").value.toString())
                binding.edtLName.setText(it.child("lname").value.toString())
                binding.edtMobileNo.setText(it.child("mobileNo").value.toString())
                binding.edtEmail.setText(it.child("emailAdd").value.toString())

                Toast.makeText(this,"Found",Toast.LENGTH_LONG).show()
            }
            else{
                Toast.makeText(this,"not found",Toast.LENGTH_LONG).show()
            }
        }.addOnFailureListener {
            Toast.makeText(this,"Something Wrong",Toast.LENGTH_LONG).show()
        }
    }
}
--------------------------------------
userdata.kt
package com.example.mcaapplication

data class UserData(
    val fName : String? = null,
    val lName : String? = null,
    val mobileNo : String? = null,
    val emailAdd : String? = null
)
---------------------------------------
database 
package com.example.mcaapplication

import android.content.Intent
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.util.Log
import android.widget.Toast
import com.example.mcaapplication.databinding.ActivityDatabaseBinding
import com.google.firebase.database.DatabaseReference
import com.google.firebase.database.FirebaseDatabase

class DatabaseActivity : AppCompatActivity() {

    private lateinit var binding : ActivityDatabaseBinding

    private lateinit var dataBaseRefrence : DatabaseReference

    var uFName: String? = null
    var uLName: String? = null
    var mobileNo: String? = null
    var email: String? = null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityDatabaseBinding.inflate(layoutInflater)
        setContentView(binding.root)

        binding.btnAdd.setOnClickListener {

             uFName = binding.edtFName.text.toString()
             uLName = binding.edtLName.text.toString()
             mobileNo = binding.edtMobileNo.text.toString()
             email = binding.edtEmail.text.toString()

            dataBaseRefrence = FirebaseDatabase.getInstance().getReference("Emp")
            val studInfo = UserData(uFName,uLName,mobileNo,email)
            dataBaseRefrence.child(mobileNo.toString()).setValue(studInfo).addOnSuccessListener {
                Toast.makeText(this,"Saved",Toast.LENGTH_LONG).show()

                binding.edtFName.text.clear()
                binding.edtLName.text.clear()
                binding.edtMobileNo.text.clear()
                binding.edtEmail.text.clear()

            }.addOnFailureListener {
                Toast.makeText(this,"Failed",Toast.LENGTH_LONG).show()
            }
        }

        binding.btnRead.setOnClickListener {
            startActivity(Intent(this,GetDataActivity::class.java))
        }

    }
}

