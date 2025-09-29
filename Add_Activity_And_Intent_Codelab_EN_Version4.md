# Android Codelab: Adding an Activity, Setting the Launcher, and Passing Data with Intents

---

## Step 1: What is an Activity?

**Theory:**  
An **Activity** is a single screen in your app. Most Android apps have multiple activities (screens) that users switch between.

---

## Step 2: Adding an Activity in Android Studio

**How-To:**
1. In Android Studio, right-click the `java` folder in your project.
2. Choose **New > Activity > Empty Views Activity**.
3. Enter a name (e.g., `PlayerNamesActivity`).
4. Android Studio creates:
    - A new Java/Kotlin file: `PlayerNamesActivity.java`
    - A layout XML file: `activity_player_names.xml`
    - Registers the activity in your `AndroidManifest.xml`:
      ```xml
      <activity android:name=".PlayerNamesActivity"/>
      ```

**Note:**  
When added, the new activity is NOT set as the launcher by default.

---

## Step 3: Setting the Launcher Activity

**Theory:**  
The **launcher activity** is the screen that appears when your app starts.  
This is controlled by the `<intent-filter>` in the manifest.

**How-To:**  
To make your new activity the launcher, update `AndroidManifest.xml`:

```xml
<activity android:name=".PlayerNamesActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN"/>
        <category android:name="android.intent.category.LAUNCHER"/>
    </intent-filter>
</activity>
```

- Only one activity should have this intent-filter.
- Remove the intent-filter from the previous launcher (usually `MainActivity`) if necessary.

---

## Step 4: Designing the New Activity UI

**Goal:**  
Let users enter two player names and click a button to continue.

**Layout (activity_player_names.xml):**

```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="32dp">

    <EditText
        android:id="@+id/etPlayer1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Player 1 Name"/>

    <EditText
        android:id="@+id/etPlayer2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Player 2 Name"/>

    <Button
        android:id="@+id/btnStartGame"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Start Game"/>
</LinearLayout>
```

---

## Step 5: Moving Between Activities with Explicit Intents

**Theory:**  
An **Intent** is a message to start another component (like an Activity).  
An **explicit intent** specifies the target Activity class directly.

**How-To:**  
In your new activity (`PlayerNamesActivity.java`):

```java
package com.example.dynamicviewadder;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import androidx.appcompat.app.AppCompatActivity;

public class PlayerNamesActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_player_names);

        EditText etPlayer1 = findViewById(R.id.etPlayer1);
        EditText etPlayer2 = findViewById(R.id.etPlayer2);
        Button btnStartGame = findViewById(R.id.btnStartGame);

        btnStartGame.setOnClickListener(v -> {
            String player1 = etPlayer1.getText().toString();
            String player2 = etPlayer2.getText().toString();

            Intent intent = new Intent(PlayerNamesActivity.this, MainActivity.class);
            intent.putExtra("player1", player1);
            intent.putExtra("player2", player2);
            startActivity(intent);
        });
    }
}
```

---

## Step 6: Receiving Data in the Next Activity

**Theory:**  
You can pass data (like the entered names) using Intent **extras**.

**How-To:**  
In `MainActivity.java`, receive the data:

```java
String player1 = getIntent().getStringExtra("player1");
String player2 = getIntent().getStringExtra("player2");
```

**Show the names:**  
Add two TextViews to your `activity_main.xml` layout and set their text in `onCreate`.

```xml
<TextView
    android:id="@+id/tvPlayer1"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="Player 1: "/>

<TextView
    android:id="@+id/tvPlayer2"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="Player 2: "/>
```

```java
TextView tvPlayer1 = findViewById(R.id.tvPlayer1);
TextView tvPlayer2 = findViewById(R.id.tvPlayer2);

if (player1 != null) tvPlayer1.setText("Player 1: " + player1);
if (player2 != null) tvPlayer2.setText("Player 2: " + player2);
```

---

## Step 7: Summary

- **You added a new Activity** in Android Studio
- **You set it as the launcher** in the manifest
- **You designed a UI** for user input
- **You used an explicit Intent to move between Activities**
- **You passed and received data using Intent extras**

**References:**  
- [Activities | Android Developers](https://developer.android.com/guide/components/activities/intro-activities)
- [Intents | Android Developers](https://developer.android.com/guide/components/intents-filters)
- [Manifest | Android Developers](https://developer.android.com/guide/topics/manifest/manifest-intro)

---