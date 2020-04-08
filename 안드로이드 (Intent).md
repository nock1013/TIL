

# 안드로이드 (Intent)

### 1. main intent

manifest activity등록 

```java
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="multi.android.intent">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".ReturnDataFirstActivity"></activity>
        <activity android:name=".ReturnDataSecondActivity"></activity>
        <activity android:name=".SecondActivity"></activity>
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

[기본실행]

1. 인텐트 객체를 생성하고 실행할 액티비티의 정보와 데이터를 셋팅
   * 값 -  putExtra() 메소드를 이용 
   * 객체

2. 안드로이드 OS에 인텐트객체 넘기며 의뢰
   * 액티비티 실행 명령어 :startActivity

3. 인텐트에 설정되어 있는 액티비티 호출 - second Activity

4. 호출된 액티비티에서는 안드로이드 OS가 넘겨준 인텐트를 가져오기

5. 인텐트에 셋팅된 데이터를 꺼내서 활용

   ​    -main Activity

   ```java
   public class MainActivity extends AppCompatActivity {
   
       @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
           setContentView(R.layout.activity_main);
           Button btn = findViewById(R.id.button);
           btn.setOnClickListener(new View.OnClickListener() {
               @Override
               public void onClick(View v) {
                   Intent intent = new Intent(MainActivity.this,
                                       SecondActivity.class);
                   //intent에 공유할 데이터 저장 - 값을 넘기는 쪽
                   intent.putExtra("info","첫 번째 액티비티가 넘기는 메세지");
                   intent.putExtra("num",10000);
   
                   startActivity(intent);
               }
           });
       }
   }
   ```

   ​	-second Activity 

   ```java
   public class SecondActivity extends AppCompatActivity {
       @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
           setContentView(R.layout.second);
           Button bt2 = findViewById(R.id.bt2);
           //인텐트 객체 추출
           Intent intent = getIntent();
           //인텐트 객체에서 공유된 값을 꺼내기 - 받는 쪽
           String msg = intent.getStringExtra("info");
           int data = intent.getIntExtra("num",0);
           Toast.makeText(this,"추출한 값:"+msg+","+data,Toast.LENGTH_LONG).show();
           bt2.setOnClickListener(new View.OnClickListener() {
               @Override
               public void onClick(View v) {
                   finish();//엑티비티 종료
               }
           });
   
       }
   }
   ```

### 2. ReturnIntent

- ReturnDataFistActivity

```java
public class ReturnDataFirstActivity extends AppCompatActivity
			implements OnClickListener{
    public static final int SECOND_BUTTON = 10;
    /** Called when the activity is first created. */
	Button bt1;
	Button bt2;
    Button bt3;
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.first2);
        bt1 = (Button)findViewById(R.id.call1);
        bt2 = (Button)findViewById(R.id.call2);
        bt3 = (Button)findViewById(R.id.btnExit);
        
        bt1.setOnClickListener(this);
        bt2.setOnClickListener(this);
        bt3.setOnClickListener(this);
        Log.d("kim","onCreate()");
    }
	@Override
	public void onClick(View v) {
		if(v.getId()==R.id.btnExit){
		    finish();
        }else if(v.getId()==R.id.call1){
		    Intent intent  = new Intent();
            intent.putExtra("info","첫 번째 액티비티가 넘기는 메시지");
            startActivity(intent);
        }else if(v.getId()==R.id.call2){
		    //새로운 액티비티를 실행해서 작업을 완료한 후 되돌아오는 작업을 수행
            Intent intent = new Intent(this,ReturnDataSecondActivity.class);
            intent.putExtra("code","call2");
            intent.putExtra("data","첫 번째 액티비티가 넘기는 메세지");
            //되돌아올때 사용되는 메소드가 startActivityForResult
            //인텐트객체와 함께 request code를 넘긴다. (사용자 정의)
            startActivityForResult(intent,SECOND_BUTTON);
        }
	}

	//인텐트를 통해서 액티비티를 호출하고 되돌아오는 경우 메소드를 추가해서 작업 자동으로
    //onActivityResult를 호출한다. - 되돌아와서 마무리해야 하므로
    // onActivityResult를 오버라이딩해서 처리할 작업을 구현
    //requestCode - 요청을 했던 뷰를 구분하기 위한 코드
    //resultCode - 결과코드
    //data - Intent객체
    @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent intent) {
        super.onActivityResult(requestCode, resultCode, intent);
        if(requestCode==SECOND_BUTTON){
            if(resultCode==RESULT_OK){
                String returndate = intent.getStringExtra("second");
                Toast.makeText(this,returndate,Toast.LENGTH_SHORT).show();
            }
        }

    }
}
```

- ReturnDataSecondActivity

```java
public class ReturnDataSecondActivity extends AppCompatActivity {
	String code;
	/** Called when the activity is first created. */
	@Override
	public void onCreate(Bundle savedInstanceState) {
	    super.onCreate(savedInstanceState);
	    setContentView(R.layout.second2);
	    Button bt1 = (Button)findViewById(R.id.btnClose1);
		final TextView txt = findViewById(R.id.secondTxt);
		final Intent intent = getIntent();
		code = intent.getStringExtra("code");
	    bt1.setOnClickListener(new OnClickListener() {
			@Override
			public void onClick(View v) {
				switch (code){
					case  "call2" :
						String data = intent.getStringExtra("data");
						txt.setText(data);

						intent.putExtra("second","두 번째 액티비티에서 실행 완료");
						//실행 후에 호출한 액티비티로 되돌아가기
						//되돌아갈때 값을 공유하기 위해 intent객체를 넘긴다.
						setResult(RESULT_OK,intent);
						finish();
				}

			}
		});
	}

}
```

