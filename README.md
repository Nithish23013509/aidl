# Ex.No:2 Develop an android application to implement the AIDL server and client app. The server app hosts a Bound Service and contains the logic to return random colours to its client.The client app calls the service and changes the button's colour within the Main activity.



## AIM:

To Develop an android application to implement the AIDL server and client app. The server app hosts a Bound Service and contains the logic to return random colours to its client.
The client app calls the service and changes the button's colour within the Main activity using AIDL interface in Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min.required Griaffe )

## ALGORITHM:

Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as CSAIDL and click Next. 

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6: Display message give in MainActivity file(client/server).

Step 7: Save and run the application.

## PROGRAM:
```
Program to print the client/server services using AIDL”.
Developed by: NITHISH S
Registeration Number : 212223220070
```
## SERVER SIDE
## MainActivity.java
```
package com.example.aidlserver;

import android.os.Bundle;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });
    }

}
```
## ColorService.java
```
package com.example.aidlserver;

import android.app.Service;
import android.content.Intent;
import android.os.IBinder;
import android.os.RemoteException;
import android.graphics.Color;
import java.util.Random;

public class ColorService extends Service {
    private final IColorService.Stub binder = new IColorService.Stub() {
        @Override
        public int getRandomColor() throws RemoteException {
            Random random = new Random();
            int red = random.nextInt(256);
            int green = random.nextInt(256);
            int blue = random.nextInt(256);
            return Color.rgb(red, green, blue);
        }
    };

    @Override
    public IBinder onBind(Intent intent) {
        return binder;
    }
}
```
## IColorService.aidl
```
package com.example.aidlserver;
interface IColorService {
 int getRandomColor();
}
```
## CLIENT SIDE
## MainActivity.java
```
package com.example.aidlclient;
import android.content.ComponentName;
import android.content.Context;
import android.content.Intent;
import android.content.ServiceConnection;
import android.os.Bundle;
import android.os.IBinder;
import android.os.RemoteException;
import android.widget.Button;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;
import com.example.aidlserver.IColorService;
public class MainActivity extends AppCompatActivity {
    private IColorService colorService;
    private boolean isBound = false;
    private Button btnChangeColor;
    private final ServiceConnection connection = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName name, IBinder service)
        {
            colorService = IColorService.Stub.asInterface(service);
            isBound = true;
            Toast.makeText(MainActivity.this, "Connected",
                    Toast.LENGTH_SHORT).show();
        }
        @Override
        public void onServiceDisconnected(ComponentName name) {
            isBound = false;
            colorService = null;
        }
    };
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        btnChangeColor = findViewById(R.id.btnChangeColor);
        btnChangeColor.setOnClickListener(v -> {
            if (isBound && colorService != null) {
                try {
                    int randomColor = colorService.getRandomColor();
                    btnChangeColor.setBackgroundColor(randomColor);
                } catch (RemoteException e) {
                    Toast.makeText(this, "Error",
                            Toast.LENGTH_SHORT).show();
                }
            } else {
                Toast.makeText(this, "Not connected",
                        Toast.LENGTH_SHORT).show();
            }
        });
    }
    @Override
    protected void onStart() {
        super.onStart();
        // Bind to the server's service
        Intent intent = new Intent("com.example.aidlserver.IColorService");
        intent.setPackage("com.example.aidlserver");
        bindService(intent, connection, Context.BIND_AUTO_CREATE);
    }
    @Override
    protected void onStop() {
        super.onStop();
        if (isBound) {
            unbindService(connection);
            isBound = false;
        }
    }
}
```
## IColorService.aidl
```
package com.example.aidlserver;

interface IColorService {
    int getRandomColor();
}
```
## activity_main.xml
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
    <LinearLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center"
        android:orientation="vertical">
        <Button
            android:id="@+id/btnChangeColor"
            android:layout_width="200dp"
            android:layout_height="200dp"
            android:text="Tap me!"
            android:textSize="24sp" />
    </LinearLayout>

</androidx.constraintlayout.widget.ConstraintLayout>
```


## OUTPUT
<img width="1919" height="1019" alt="image" src="https://github.com/user-attachments/assets/19f4e0cb-9361-4f5b-8e00-501104083429" />
<img width="1919" height="1017" alt="image" src="https://github.com/user-attachments/assets/24814fd3-1150-4a96-b823-3cf3483f03ae" />
<img width="1919" height="1025" alt="image" src="https://github.com/user-attachments/assets/7e8169d3-be7f-45c7-8dda-25b955696538" />





## RESULT
Thus a Simple Android Application to create a AIDL interface and communicate the process between client and server using AIDL interface in Android Studio is developed and executed successfully.
