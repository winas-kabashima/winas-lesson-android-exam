
## Day1

**（１）モバイルアプリを開発する上で、設計上留意すべき点はどこになるか、サーバサイドやフロントエンドとの違いの観点から説明してください。**

**（２）Activityへの過度な依存や類似/同一コードの重複を避けるため、コード設計上どのような対策をとることが望ましいか、プレゼンテーション層（View）と処理・ビジネスロジック（Controller）それぞれの観点から、実際のコード例を挙げて説明してください。**

## Day2

**（３）以下のそれぞれのレイアウトを実現するレイアウトXMLを書いてください。**

①　横一列に３つのViewを並べるレイアウト。その際、３つのViewの幅(width)の比率が、`3:2:1`になるようにすること。また、それぞれ左右に`10dp`ずつマージンを取り、高さはすべて`100dp`とすること。
①回答
```
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="horizontal">
    <View
        android:layout_width="0dp"
        android:layout_weight="3"
        android:layout_height="100dp"
        android:layout_marginStart="10dp"
        android:layout_marginEnd="5dp"/>
    <View
        android:layout_width="0dp"
        android:layout_weight="2"
        android:layout_height="100dp"
        android:layout_marginStart="5dp"
        android:layout_marginEnd="5dp"/>
    <View
        android:layout_width="0dp"
        android:layout_weight="1"
        android:layout_height="100dp"
        android:layout_marginStart="5dp"
        android:layout_marginEnd="10dp"/>
</LinearLayout>
```

②　画面いっぱい（上下左右に`10dp`ずつマージン）にViewを置き、それに重ねる形で、width `100dp`、height `100dp`のViewを縦横中央にそろえたレイアウト。色はそれぞれ変えること。
②回答
```
<FrameLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
        <View
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_marginTop="10dp"
            android:layout_marginBottom="10dp"
            android:layout_marginStart="10dp"
            android:layout_marginEnd="10dp"
            android:background="#FF0000"/>
    </LinearLayout>
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">
        <View
            android:layout_width="100dp"
            android:layout_height="100dp"
            android:layout_gravity="center"
            android:background="#00FF00"/>
    </LinearLayout>
</FrameLayout>
```

③　親ビューの下部に横幅`100%`、高さは可変サイズのTextViewを配置し、その上部にwidth `100dp`、height `100dp`のViewを左右の中央にそろえたレイアウト。
③回答
```

```


**（４）以下のコードのTODO箇所を埋める形で、ActivityのContentViewの各Viewコンポーネントをプロパティアクセスで参照するコードを、①findViewByIdを使う方法、②ViewBindingを使う方法それぞれで書いてください。
その際、下記の要件に従うこと。**

> - レイアウトファイルの名前は、`main_activity.xml`とする

```kotlin
class MainActivity : Activity() ViewBindable{
    override lateinit var binding: ViewBinding
    private val findViewByIdView: View?
        get() = findViewById<View>(R.id.view)
        // TODO: ①findViewByIdからViewを取得
    private val viewBindingView: View?
        get() = (binding as? MainActivityBinding)?.view
        // TODO: ②ViewBindingからViewを取得

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // TODO: ここでContentViewを設定すること。①②それぞれでコードを書くこと
        // ①
        setContentView(R.layout.main_activity)

        // ②
        val binding = MainActivityBinding.inflate(layoutInflater)
        val view = binding.root
        setContentView(view)

    }
}
interface ViewBindable {
    var binding: ViewBinding
}
```

## Day3

**（５）以下のコードのTODO箇所を埋める形で、①Activityが新しい別のActivityを呼び出した際にデータを渡し、②呼び出されたActivityが閉じられる際に古いActivityにデータを渡すコードを書いてください。なお、コード中に使われるIDコードは、コード内にあるenumを使用すること。**

```kotlin
data class Content(
    var id: Int = 0,
    var name: String = ""
) : Parcelable {
    override fun writeToParcel(parcel: Parcel, flags: Int) {
        parcel.writeInt(id)
        parcel.writeString(name)
    }
    override fun describeContents(): Int {
        return 0
    }
    constructor(parcel: Parcel) : this(
        parcel.readInt() ?: 0,
        parcel.readString() ?: "")
    companion object CREATOR : Parcelable.Creator<Content> {
        override fun createFromParcel(parcel: Parcel): Content {
            return Content(parcel)
        }
        override fun newArray(size: Int): Array<Content?> {
            return arrayOfNulls(size)
        }
    }
}
enum class ActivityRequestCode(val code: Int) {
    TARGET(11111);
}
enum class ActivityResultCode(val code: Int) {
    TARGET(1111);
}
enum class ActivityExtraData(val key: String) {
    NUMBER("number"), CONTENT("content");
}
class MainActivity : Activity() {
    private val button: Button?
        get() = findViewById<Button>(R.id.button) as? Button
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        button?.setOnClickListener {
            // TODO: Contentデータを設定してTargetActivity呼び出し
        }
    }
    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        // TODO: TargetActivityから返されたデータをデバッグ
    }
}
class TargetActivity : Activity() {
    private val closeButton: Button?
        get() = findViewById<Button>(R.id.button) as? Button
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_target)
        // TODO: MainActivityから渡されたデータを復元
        
        closeButton?.setOnClickListener {
            // TODO: MainActivityに返す任意のデータ（Int）を設定してください
            finish()
        }
    }
}
```

**（６）以下のコードのTODO箇所を埋めて、Activity（呼び出し側）とカスタムビューとの間での処理のやりとりを実現するコードを書いてください。その際、下記の条件を満たすこと。**

> - `SampleView`に`listener`を設定して、ボタンがタップされた際にUIViewController側でイベントを受け取るようにすること
> - `SampleView`にデータ（`Content`）を設定したら、`textView`にデータの`text`を表示させること
> - `SampleView`の`update()`関数を呼び出したら、`textView`に表示される情報を更新すること

```kotlin
data class Content(
    var id: Int = 0,
    var text: String = ""
)
class MainActivity : Activity() {
    private val view: SampleView?
        get() = findViewById<SampleView>(R.id.sample_view) as? SampleView
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        val content = Content(id = 1234, text = "test content")
        // TODO
    }
}
class SampleView : FrameLayout {
    private val textView: TextView?
        get() = findViewById<TextView>(R.id.text_view) as? TextView
    private val button: Button?
        get() = findViewById<TextView>(R.id.text_view) as? TextView
    constructor(context: Context) : super(context) {
        // TODO
    }
    // TODO
}
```

**（７）以下のコードのTODO箇所を埋めて、コード中の各ImageViewに、指定された方法で画像を表示させるコードを書いてください。その際、下記の条件を満たすこと。**

> - `imageView1`にresourceのdrawableから "icon" を設定すること
> - `imageView2`にassetから画像ファイル "icon2.png" を読み込み、Bitmapとして設定すること
> - `imageView3`にenum`BrandIcon`を使ってアイコンフォントの画像を設定すること
> - `imageView4`にOSS`Picasso`を使って、画像URL "https://sample.com/sample.jpg" を読み込むこと

```kotlin
class MainActivity : Activity() {
    private val imageView1: ImageView?
        get() = findViewById<ImageView>(R.id.image_view_1) as? ImageView
    private val imageView2: ImageView?
        get() = findViewById<ImageView>(R.id.image_view_2) as? ImageView
    private val imageView3: ImageView?
        get() = findViewById<ImageView>(R.id.image_view_3) as? ImageView
    private val imageView4: ImageView?
        get() = findViewById<ImageView>(R.id.image_view_4) as? ImageView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        // TODO
    }
}
enum class BrandIcon(val key: Int) {
    TWITTER(1);
    val resourceId: Int
        get() {
            return when (this) {
                TWITTER -> R.string.icon_brand_twitter
            }
        }
    val color: Int
        get() {
            return when (this) {
                TWITTER -> Color.parseColor("#55acee")
            }
        }
}
abstract class FontIconDrawable: Drawable() {
    protected var text: String = ""
    protected val context: Context = App.context
    protected val textPaint: TextPaint = TextPaint()
    protected abstract val typeface: Typeface?
    protected var size: Int = 20
    protected var _alpha: Int = 255
    override fun isStateful(): Boolean {
        return true
    }
    override fun setState(stateSet: IntArray): Boolean {
        val oldValue = textPaint.alpha
        val newValue = if (isEnabled(stateSet)) _alpha else _alpha / 2
        textPaint.alpha = newValue
        return oldValue != newValue
    }
    fun isEnabled(stateSet: IntArray): Boolean {
        for (state in stateSet)
            if (state == android.R.attr.state_enabled)
                return true
        return false
    }
    fun alpha(alpha: Int): FontIconDrawable {
        setAlpha(alpha)
        invalidateSelf()
        return this
    }
    override fun draw(canvas: Canvas) {
        textPaint.textSize = bounds.height().toFloat()
        val textBounds = Rect()
        val textValue = text
        textPaint.getTextBounds(textValue, 0, 1, textBounds)
        val textBottom = (bounds.height() - textBounds.height()) / 2f + textBounds.height() - textBounds.bottom
        canvas.drawText(textValue, bounds.width() / 2f, textBottom, textPaint)
    }
    override fun setAlpha(p0: Int) {
        _alpha = p0
        textPaint.alpha = p0
    }
    override fun getOpacity(): Int {
        return PixelFormat.OPAQUE
    }
    override fun setColorFilter(p0: ColorFilter?) {
        textPaint.colorFilter = p0
    }
    override fun getIntrinsicHeight(): Int {
        return size
    }
    override fun getIntrinsicWidth(): Int {
        return size
    }
    protected fun update() {
        textPaint.typeface = typeface
        textPaint.textSize = size.toFloat()
        textPaint.style = Paint.Style.FILL_AND_STROKE
        textPaint.textAlign = Paint.Align.CENTER
        textPaint.isUnderlineText = false
        textPaint.isAntiAlias = true
        setBounds(0, 0, size, size)
        invalidateSelf()
    }
}
class BrandIconDrawable: FontIconDrawable() {
    override val typeface: Typeface? = CachedTypeFaces.get(context, "fa-brands-400.ttf")
    companion object {
        fun create(context: Context, brand: BrandIcon, size: Int): BrandIconDrawable {
            val drawable = BrandIconDrawable()
            drawable.text = context.getString(brand.resourceId)
            drawable.size = size
            drawable.update()
            drawable.textPaint.color = brand.color
            return drawable
        }
    }
}
object CachedTypeFaces {
    private val cache = HashMap<String, Typeface>()
    fun get(context: Context, assetPath: String): Typeface? {
        synchronized(cache) {
            try {
                if (!cache.containsKey(assetPath)) {
                    val typeface = Typeface.createFromAsset(context.assets, assetPath)
                    cache[assetPath] = typeface
                    return typeface
                }
            } catch (e: Exception) {
                return null
            }
            return cache[assetPath]
        }
    }
}
```

## Day4

**（８）下記の各データを保存するとき、どのような手法を用いて要件を満たせば良いか、その理由も含めて説明してください。**

①　アプリのインストール後チュートリアルの閲覧が完了したかどうかのフラグ

②　ログインが必要なアプリで、次回起動時にログインを省略するためのAPIキー

③　マスターデータ

④　ユーザーまたはアプリ運営者が継続的に投稿しているコンテンツ

⑤　④のコンテンツのキャッシュ

**（９）以下の要件を満たすデータ群を、enumを用いて実際のコードで書いてください。**

> - 名前は`Gender`とすること
> - 「ID」はenumを**定義した時点で**プロパティアクセスできるようにすること
> - `name`プロパティを呼ぶことで「名前」を呼び出せること
> - `convert()`関数を呼ぶことで、「男性」の場合は「女性」、「女性」の場合は「男性」、「不明」の場合は「不明」のenum変数を返すようにすること
> - 値の仕様は以下のテーブルの通り

|  | 男性 | 女性 | 不明 |
|---:|:---|:---:|---:|
|ID|1 |2 |0 |
|名前 |男性 |女性 |不明 |

```kotlin
// TODO : ここにコードを記述してください
```

**（１０）以下のURLのJSONをdata classに直したコードを書いてください。data classが複数ある際、それぞれの役割について簡単な説明をコメントで入れてください。**

> http://cs367.xbit.jp/~w065038/app/winas/day5/list-with-error-code.json

```kotlin
// TODO : ここにコードを記述してください
```

**（１１）データの取得と加工を、それが必要とする箇所（Activity側など）ではなく、仲介クラスやメソッド（講座では`Repository`という名前をつけたクラスを使った）を使って行ったほうが良い理由をわかりやすく説明してください。**

**（１２）以下の要件を満たすデータをAndroidXのRoomでローカル保存するために必要なコードを「すべて」書いてください。**

> - データ名 `Content`、パラメータは以下を持つものとする
> - データの保存と取得のためのコードも書くこと（呼び出すだけで処理できるように）

| パラメータ名 | 型 | Roomのカラム名 |
|---:|:---:|:---:|
|id|Int|id|
|name|String|name|
|genderId|Int|gender_id|

```kotlin
// TODO : ここにコードを記述してください
```

**（１３）サーバAPIからJSONデータを取得して、モデルクラスの形で呼び出し元まで返す過程を、準備のための実装フローも含めて、箇条書きでできるだけ詳しく、ロジックフローで説明してください。なお、ライブラリはRetrofit, OkHttp, Moshiを使うものとします。**

>例：　Step1: XXクラスをXXライブラリの仕様に沿うよう、XXする。  
>　　　Step2: XXデータをXXライブラリのXXメソッドを使ってXXする。

## Day5

**（１４）複数の異なる型を持つデータ群を１つの配列で持ちたいとする。それらのデータをサーバAPIから取得した場合、どのように実装をすればいいのか、MoshiのPolymorphicJsonAdapterFactoryを使って、以下のコードのTODO箇所を埋める形でコードを書いてください。**

>  - APIのURLはこちらから `http://cs367.xbit.jp/~w065038/app/winas/day5/list-adr.json`

```kotlin
val apiInterface = Retrofit
    .Builder()
    .addConverterFactory(
        MoshiConverterFactory.create(
            Moshi.Builder()
                // TODO
                .add(KotlinJsonAdapterFactory())
                .build()
        )
    )
    .addCallAdapterFactory(CoroutineCallAdapterFactory())
    .client(createOkHttpClient())
    .baseUrl("http://cs367.xbit.jp/~w065038/app/winas/day5/")
    .build()
    .create(SampleApiService::class.java)
data class Restaurant(
    var id: Int = 0,
    var name: String = ""): Feedable {}
data class Hospital (
    var id: Int = 0,
    var name: String = ""): Feedable {}
enum class FeedContentType(val index: String) {
    Hospital("hospital"), Restraunt("restaurant")
}
interface Feedable {
    val feedContentType: FeedContentType
}
interface SampleApiService {
    // TODO: 任意のサンプル用APIを定義すること
}
fun getList(
    completion: (contents: List</* TODO */>) -> Unit,
    failure: () -> Unit
    ) {
    // TODO: ここで実際にAPIを呼んでデータに変換する
}
getList(
    completion = { contents ->
        // TODO: ここで結果が返るようにする
    }, failure = {
        // do nothing
    }
)
```

**（１５）あるカスタムビュークラスが、プロパティのデータの型によって異なる表示を行うものとします。その際、ジェネリクスを使った場合の実装、使わない場合の実装のコード例を、以下のコードを埋める形でそれぞれ書いてください。**

```kotlin
data class Dog(
    override var id: Int = 0,
    override var name: String = "いぬ🐶"): Animal {}
data class Cat(
    override var id: Int = 0,
    override var name: String = "ねこ🐱"): Animal {}
interface Animal {
    val id: Int
    val name: String
}
class SampleCustomView: FrameLayout {
    var data: ??? // TODO: 型名、textViewに`name`を表示させること
    private val textView: TextView?
        get() = findViewById<TextView>(R.id.text_view) as? TextView
}
```
**（１６）あるActivityが、プロパティに指定されたモデルデータに基づいてUIを構築・表示する役割を持っていた場合について考えます。**

①　その「モデルデータ」がnullになるケースとしてどのようなケースが考えられるか。わかりやすく説明してください。

②　モデルデータがnullであった場合、どのようにして代替処理を行うか、以下のコードのTODO箇所を埋める形でコードを書いてください。

```kotlin
data class Content(
    var id: Int = 0,
    var text: String = ""
)
class ContentActivity : Activity() {
    var content: Content? = null
    // TODO : contentが設定されない場合の代替処理
    
    private val textView: TextView?
        get() = findViewById<TextView>(R.id.text_view) as? TextView
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_content)
        // TODO : contentを取得してnullの場合の代替処理
    }
}
class ContentRepository {
    fun getHospital(
        hospitalId: Int,
        completion: (hospital: Hospital) -> Unit,
        failure: () -> Unit
    ) {
        // API通信でのデータ取得処理は省略
        let content = Content()
        content.id = 1234
        content.name = "テストコンテンツ"
        completion(content)
    }
}
```

## Day6

**（１７）Activityが破棄されてから復元された際に、プロパティ「counter」のデータも復元されるよう、以下のコードに追記する形で実装してください。**

```kotlin
class SampleActivity : Activity() {
    private var counter: Int = 0
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }
}
```

