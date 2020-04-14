# 안드로이드(Fragment)



1. add메소드를 이용하면 프레그먼트 객체를 생성한다.// => add는 똑같은 객체를 하나만 생성할 수 있다. 

    * LinearLayout에 추가하면 레이아웃의 특성 상 영역 바깥에 추가되어 안보임

      

2. replace는 있으면 있는 객체를 연결, 없으면 새로 생성해서 연결

   ```java
   transaction.replace(R.id.container,firstFragment);
   ```



3. 프레그먼트는 액티비티의 lifcycle에 종속적이나 액티비티처럼 동작할 수 있도록backstack에 등록할 수 있다.

    * addToBackStack을 이용

      ```java
      transaction.addToBackStack("first");
      ```













----

---

```java
@Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        Log.d("lifecycle","fragment======onCreateView");
        return inflater.inflate(R.layout.fragment_first2, container, false);
    }

    @Override
    public void onAttach(@NonNull Context context) {
        super.onAttach(context);
        Log.d("lifecycle","fragment======onAttach");

    }

    @Override
    public void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        Log.d("lifecycle","fragment======onCreate");

    }

    @Override
    public void onViewCreated(@NonNull View view, @Nullable Bundle savedInstanceState) {
        super.onViewCreated(view, savedInstanceState);
        Log.d("lifecycle","fragment======onViewCreated");

    }

    @Override
    public void onActivityCreated(@Nullable Bundle savedInstanceState) {
        super.onActivityCreated(savedInstanceState);
        Log.d("lifecycle","fragment======onActivityCreated");
    }

    @Override
    public void onStart() {
        super.onStart();
        Log.d("lifecycle","fragment======onStart");

    }

    @Override
    public void onResume() {
        super.onResume();
        Log.d("lifecycle","fragment======onResume");

    }

    @Override
    public void onPause() {
        super.onPause();
        Log.d("lifecycle","fragment======onPause");

    }

    @Override
    public void onStop() {
        super.onStop();
        Log.d("lifecycle","fragment======onStop");

    }

    @Override
    public void onDestroyView() {
        super.onDestroyView();
        Log.d("lifecycle","fragment======onDestroyView");

    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        Log.d("lifecycle","fragment======onDestroy");
    }

    @Override
    public void onDetach() {
        super.onDetach();
        Log.d("lifecycle","fragment======onDetach");

    }
```