

◎概要
OpenCvを.Net Framework上から使用する為のクラスライブラリです。
主にVB(VB2008)から使用することを目的に作成してありますが、
C#などからも使用できると思います。
（普段C#を使っていないので、試された方、状況を教えてもらえるとうれしいです）

同様のライブラリはいくつかあるようですが、
このライブラリの特徴と言えるものは下記になります。

・OpenCVのソース(ヘッダファイル)から
　ライブラリ用のソースを自動生成しています。
　（ただし例外的に手動で変換している部分はあります。
　　また変換できない部分はバッサリとカットしています。）

・オリジナルにできるだけ近い使い方ができるように考えています。
　よってオブジェクト指向な使われ方はあまり考慮されていません。
　（オブジェクト指向なプログラミングを信条としている方は
　　別途ラッパするライブラリ等を作ってくださいませ。）

・速度よりも互換性を重視しています。
　速度が必要な場合はオリジナルのOpenCVをC/C++から使うべきです。

・以前のバージョン(OpenCVLib.dll)はVBで作成されていましたが、
　こちらのバージョン(OpenCVxxx.dll)はC++/CLIで作成されています。
　またDLLファイル名を推測できるものにしています。
　（Ver1.0用 => OpenCV100.DLL、Ver1.1用 => OpenCV110.DLL）


◎ダウンロード／インストール


ライブラリ版には、クラスライブラリのDLL(OpenCVxxx.DLL)と
動作確認用のサンプルプログラムが入っています。

ソース版には、ライブラリ版の内容に加え、
DLL及びサンプルプログラムの全ソースファイルが含まれます。


DLLを使用する場合は、上記のライブラリ版を適当なフォルダに展開し、
VB上からDLLを参照設定（メニュー「プロジェクト」－「参照の追加」）して下さい。

DLLへの参照が追加されると
「ManagedOpenCV」という名前空間(クラス)が利用できるようになります。

なお当然ですが、別途OpenCVがインストールされている必要があります。


◎ライセンス
ライセンスはOpenCVのライセンスに従います。
GPLではないので、OpenCVを使用していることを明記すれば、
自前のプログラムに組み込んで使用してもソースを公開する義務はないと思います。


---------------------------------------------------------------

◎使用上の注意

○サポートされているもの
関数、定数、構造体はほとんどが使用可能です。
コールバックの型定義はデリゲートとして定義されています。

ただし概要にも書いてありますが、基本的に自動変換によって生成していますので
引数の型などが不適切なものがあるかもしれません。
掲示板かメールで連絡頂ければできるだけ修正いたします。

引数付きマクロについては、ほとんどサポートしておりません。
言語の差異により自動での変換が困難なためで、
使用頻度の高いものなどは手動で追加してあります。
（CV_IABS、CV_RGB、CV_NEXT_SEQ_ELEM、などはサポートされます）

関数、定数、構造体、などのサポート状況については
DLLソースフォルダ下にある「report.txt」を参照してください。



○名前空間について
全ての関数/定数などは「ManagedOpenCV」というクラスの静的メンバとして定義されています。
ですので、各ソースファイルの先頭で
　Imports ManagedOpenCV
と記述しておけば、普通の関数のように呼び出すことが出来ます。



○関数/定数/マクロの名称について
原則としてオリジナルと同じ名前で使用できます。
ただし、VBでは名称の大文字小文字を区別しない為、
関数名と構造体名が(大文字小文字を無視すると)同じであった場合問題が発生します。
この為構造体名と同じ名称の関数は、名前の後に「_」(アンダーバー)が付いた名前になっています。
（出来るだけ見た目を同じにしたいが為の処置です。）

　例）関数　　cvSize()　=>  cvSize_()	後に '_' が付く
　　　構造体　CvSize    =>  CvSize		同じ名前

OpenCVの構造体を初期化して返す関数(cvSizeやcvScalar等)は
構造体の名称を小文字化した名称になっていますので
その場合は関数の後に「_」を追加するようにしてください。
また、一部の構造体には同等の動作をするコンストラクタを追加しています。
（CvMat、CvRect、CvPoint、CvSize、など。）

　例）img = cvCreateImage( cvSize(sx,sy), 8, 1 )
　　　　　　　　　↓
　　　img = cvCreateImage( cvSize_(sx,sy), 8, 1 )
　　　　　　　　　または
　　　img = cvCreateImage( new CvSize(sx,sy), 8, 1 )

機能的には一緒です（コンストラクタは初期化関数を呼び出してます。）



○ユーティリティ関数
OpenCVには存在しませんが、あると便利な関数として下記を追加しています。

　Function GetOpenCVInstallDirectory() as String
　　OpenCVがインストールされているフォルダを返します。
　　サンプル画像を使用する場合は、
　　　GetOpenCVInstallDirectory() + "\samples\c\lena.jpg"
　　などとしてください。

　Function LoadImageFromResorce( ByVal ResourceName As String ) As IplImage
　Function LoadImageFromResorce( ByVal ResourceName As String, Byval iscolor as Integer ) As IplImage
　　リソースの画像ファイルを読み込み、イメージデータ(IplImage)を作成します。
　　リソースデータはビルドアクションを「組み込まれたリソース」にして下さい。

　　　例）リソースとしてimg.bmpをプロジェクトに追加した場合
　　　　　Dim img As IplImage = LoadImageFromResorce( "img.bmp" )

　　iscolorにCV_LOAD_IMAGE_GRAYSCALEが指定された場合、１チャンネル画像となります。
　　iscolorにCV_LOAD_IMAGE_COLORが指定された場合、３チャンネル画像となります。
　　iscolorを省略又は上記以外の場合、元画像に合わせたチャンネル数となります。


○構造体について
全てのOpenCV(オリジナル)での構造体(以下CV構造体)はクラスとして定義されています。
これは構造体のデータはできるだけアンマネージ領域においたままにして
アンマネージ/マネージ間のデータ変換を極力抑え
GCの影響を受けないようにしたかった為です。

しかしこれによってCでの記述とVBでの記述で大きく異なる部分が出てきます。
（考え方としては「全てポインタ変数になっている」と思えばよいかもしれません。）
下記の点に注意して下さい。

・VBでCV構造体(正確にはクラス)の変数を定義しただけでは実体が作られていません。
　→　Newで新規にインスタンスを生成してください。
　　　なおアンマネージ領域に新たに領域を作成する方法と、
　　　既存のアンマネージ領域をラップする方法があります。（後述）
　　　　例）CvPoint p1;		→	Dim p1 As New CvPoint
　　　　　　CvPoint *p2;	→	Dim p2 as CvPoint
　　　　　　CvPoint *p3 = (CvPoint*)ptr;	
							→	Dim p3 as CvPoint = New CvPoint(ptr)

・C/C++では構造体を代入すると全てのメンバがコピーされますが、
　VBではクラスであるため参照情報のみコピーされ、実際の値はコピーされません。
　値を代入(コピー)したい場合は、CV構造体のCopyFromメソッドを使用してください。
　　　　例）CvPoint a,b;	→		Dim a As New CvPoint, b As CvPoint
　　　　　　a = b;					a.CopyFrom( b )

　ただし、構造体の中の配列に関しては、代入することで値がコピーされます。
　（CvRandStateのparamなど）

CV構造体を定義する場合、
何も引数を与えなければアンマネージ領域に新しい領域を確保します。
この場合、CV構造体(クラス)が破棄されるとき自動的にアンマネージの領域も破棄されます。
C言語での動作と同様に、構造体の各メンバはクリアされません。

アンマネージポインタを指定して生成した場合は、
その領域にある構造体をラップするオブジェクトとなります。
この場合オブジェクトを破棄してもアンマネージ領域は解放されません。

ほとんどのCV構造体には下記のメソッド/プロパティが用意されています。
（構造体内部の名前なし構造体などには存在しない場合もあります。）

　StructSizeプロパティ
　　CV構造体のバイト数を返します。
　　このプロパティは静的メンバです。
　　　例）Dim ptr As IntPtr = Marshal.AllocHGlobal( CvSize.StructSize )

　CopyFromメソッド
　　他のCV構造体の内容をコピーします。
　　　例）a.CopyFrom( b )	ｂの内容がａにコピーされます。

　Cloneメソッド
　　自身の複製を作成します。

　GetArrayメソッド
　　自身のアンマネージ領域を先頭とする構造体配列を返します。
　　引数として配列の要素数を指定します。
　　　例）Dim pnt() As CvPoint = ret.GetArray( 5 )
	静的メンバ版では、ポインタと個数を指定します。
　　　例）dim pnt() As CvPoint = CvPoint.GetArray( ptr, 5 )

　CreateNewArrayメソッド
　　連続したアンマネージ領域をラップするCV構造体の配列を生成します。
　　引数として配列の要素数を指定します。
　　このメソッドは静的メンバです。
　　　例）Dim pnt() As CvPoint = CvPoint.CreateNewArray( 5 )

　　通常のVB構文(Redim等)で作成した配列はアンマネージ領域が連続しません。
　　構造体配列を引数として必要とする関数には
　　必ずCreateNewArrayメソッドで生成した配列の先頭要素を指定して下さい。

全てのCV構造体はUMemoryクラスを継承していますので、
UMemoryのメソッド/プロパティも使用できます。
（AddressプロパティでCV構造体のポインタが取得できます）


○構造体、又は、構造体のポインタについて
関数の引数がCvSizeやCvPoint*などの構造体/構造体ポインタの場合、
CV構造体をそのまま指定して下さい。
CvArr*の引数にも構造体をそのまま指定して下さい。
　例）Dim sz As New CvSize( 320, 240 )
　　　Dim img as IplImage = cvCreateImage( sz, 8, 1 )	'第一引数がCvSize
　　　cvResetImageROI( img )							'第一引数がIplImage*

ダブルポインタ(IplImage**)を引数に持つものもそのまま指定して下さい。
　例）cvReleaseImage( img )			'第一引数がIplImage**



○ポインタによるメモリ操作について
VBではポインタという概念がありません。
ポインタを渡す、或いは、ポインタで受け取る、ポインタの示す位置からデータを取り出す、
といったことを直接行うことが出来ません。

その為、下記の様なクラス/構造体を用意してあります。

UPointer構造体
　アンマネージメモリのアドレス（ポインタ）を格納します。
　IntPtrとほぼ同じ意味になります。

UMemoryクラス
　アンマネージメモリの領域を管理します。
　新規に割り当てたり、既に確保されているポインタをラップして
　VBからアンマネージメモリに対して読み書きができるようにします。

U***Arrayクラス(UIntArray,UFloatArray、など）
　UMemoryと同様にアンマネージメモリの領域を管理しますが、
　特定の型の配列としてアクセスしやすいようになっているクラスです。


○UPointer構造体について
アンマネージメモリのアドレス（ポインタ）が格納されます。
IntPtrとほぼ同じようなものですが、変換や演算などが容易にできるようになってます。

下記のメソッド/プロパティがあります。

　コンストラクタ
　UPointer()	　　		0(NULL)で初期化します。
　UPointer( IntPtr Ptr )	指定されたアンマネージ領域アドレス(ポインタ)で初期化します。

　Ptrプロパティ
　　アンマネージ領域のアドレス（ポインタ）を取得・設定します。

　IsNullプロパティ
　　NULL(=0)かどうかを判断します。

　ToInt32メソッド
　　Integer型に変換したアドレスを返します。

　Nullプロパティ
　　NULLポインタを示すUPointerインスタンスを返す静的メンバです。

　+演算子/-演算子
　　整数を加算/減算します。

　=演算子/<>演算子
　　値の一致/不一致を判断します。

　IntPtr変換
　　IntPtr型へ変換、及び、IntPtr型から変換することができます。


○UMemoryクラスについて
アンマネージメモリの確保/破棄、読込/書き込みを行うクラスです。

下記のメソッド/プロパティがあります。

　コンストラクタ
　UMemory( int AllocSize )						指定されたサイズの新しい領域を確保します。
　UMemory( IntPtr Ptr, int MemorySize )			指定された領域を管理します。
　UMemory( UPointer Ptr, int MemorySize )		指定された領域を管理します。
　UMemory( UPointer Ptr )						指定された領域を管理します。
　UMemory( String^ Text )						新しい領域を確保し文字列を格納します。
　UMemory( System::Type^ Typename, int Count )	型と個数を指定し新しい領域を確保します。

　Addressプロパティ
　　先頭アドレス（ポインタ）を返します。

　Sizeプロパティ
　　領域のサイズ(バイト数)を返します。
　　サイズが不明な場合は0が返されます。

　MustDeleteプロパティ
　　クラスが破棄されるときに
　　アンマネージ領域も破棄するかどうかを取得・設定します。
　　基本的に新規作成した場合はTrue、
　　ポインタを指定して生成した場合はFalseになっています。

　MoveNextメソッド
　　Sizeプロパティの分だけポインタを進めます。
　　MustDeleteがTrueの場合は失敗します。

　****Valueプロパティ（IntValue/FloatValue等）
　　ポインタの示す位置からそれぞれの型でデータの読み書きを行います。
　　****には下記の型を指定できます。()内はVBでの型です。
　　　Byte(Byte)　		
　　　Word(UShort)		
　　　DWord(UInteger)
　　　SByte(SByte)
　　　Short(Short)
　　　Int(Integer)
　　　Float(Single)
　　　Double(Double)
　　　IntPtr(IntPtr)
　　　Address(UPointer)

　　　例）x = mm.IntValue + 1
　　　　　mm.SingleValue = 10

　****Arrayプロパティ（IntArrya/FloatArray等）
　　ポインタの示す位置を先頭とし
　　それぞれの型の配列としてデータの読み書きを行います。
　　****Valueプロパティと同じ型が指定できます。

　　　例）x = mm.IntArray( 10 )
　　　　　mm.SingleArray( 2 ) = 1.0f

　　インデックスが0の場合、****Valueと同じとなります。

　ReadTo****Arrayメソッド（ReadToIntArray/ReadToFloatArray等）
　　ポインタが示す位置を先頭とし、
　　それぞれの型の配列にデータを読み出します。
　　引数には要素数を指定します。
　　****には下記の型を指定できます。
　　　Byte(Byte)
　　　SByte(SByte)
　　　Short(Short)
　　　Int(Integer)
　　　Float(Single)
　　　Double(Double)
　　　IntPtr(IntPtr)
　　（Word、DWord、及び、Addressには使用できません）

　　　例）Dim aa() As Integer = mm.ReadToIntArray( 10 )

　　大量のデータを処理する場合、
　　マネージの配列に一括して読み込み、
　　処理が終了したのち一括して書き込みを行った方が
　　若干パフォーマンスが上がります。

　WriteFrom****Arrayメソッド（WriteFromIntArray/WriteFromFloatArray等）
　　ポインタが示す位置を先頭とし、
　　それぞれの型の配列データを書き込みます。
　　ReadTo****Arrayメソッドと同じ型が使用できます。

　　　例）Dim dd(5) As Integer
　　　　　mm.WriteFromIntArray( dd )

　PeekStringメソッド
　　ポインタが示す領域から文字列を読み込みます。
　　文字列はANSI文字列として変換されます。
　　引数を指定しない場合はNULL終端(0)が現れるまで読み込みます。
　　文字列長を指定した場合はその長さまでを読み込みます。
　　静的メンバのPeekStringメソッドでは
　　先頭アドレスを指定して読み込むことができます。

　　　例）ss = mm.PeekString()
　　　　　ss = UMemory.PeekString( Ptr, 5 )



○U****Arrayクラス(UIntArray,UFloatArray、など）
　UMemory同様にアンマネージメモリの確保/破棄、読込/書き込みを行うクラスで、
　各型の配列アクセスがデフォルトインデックスプロパティとなっています。
　****には下記の型を指定できます。
　　　Byte(Byte)　		
　　　Word(UShort)		
　　　DWord(UInteger)
　　　SByte(SByte)
　　　Short(Short)
　　　Int(Integer)
　　　Float(Single)
　　　Double(Double)
　　　IntPtr(IntPtr)
　　　Address(UPointer)

　　例）Dim tbl As New UIntArray( ptr ) 'ptrはUPointer
　　　　ans = tbl( 5 )
　　　　tbl( 5 ) = 0

　ポインタを指定して作成した場合、配列のサイズは指定できません。
　インデックスに注意しないと不正アクセスとなります。

　引数に整数を指定した場合、要素数とみなされ
　アンマネージ領域にメモリを確保します。
　　例）Dim fdata As UFloatArray( 5 )
		ans = fdata( 3 )
		fdata( 3 ) = 0.0f

　引数に対象型の配列を指定した場合、
　指定された配列の要素数分のアンマネージ領域が確保され、
　配列のデータがコピーされます。
　これによりアンマネージ領域に定数テーブルなどを直接生成することが可能です。
　　例）Dim fdata as UFloatarray( new Single() { 10.0f, 20.0f, 30.0f } )



○CvArrについて
OpenCVの関数にはCvArr型の引数をとるものが数多くあります。
オリジナルではIplImageやCvMatなど様々な構造体のポインタを指定することができます。
ライブラリでは全てUMemory型の引数として定義されていますので、
全てのCV構造体(クラス)を指定することができます。
（指定することは出来ますが、処理されるかどうかはそれぞれの関数次第です。）



○コールバックについて
コールバック関数を指定する関数(cvCreateTrackbarなど)では
オリジナルの型と同名称なデリゲートを指定します。
デリゲートを指定するときはデリゲートの生存期間に注意して下さい。
通常は下記の様に使用して下さい。

　Sub OnTrack( int Pos )
　　...
  End Sub
　Dim OnTrack_Ptr As New CvTrackbarCallback( AddressOf OnTrack )

　Sub Init()
　　...
　　cvCreateTrackbar( TRACKBARNAME, WINDOWNAME, UPointer.Null, 100, OnTrack_Ptr )
　　...
　End Sub

cvCreateTrackbarの第五引数で、
　　cvCreateTrackbar( TRACKBARNAME, WINDOWNAME, UPointer.Null, 100, AddressOf OnTrack )
と指定してしまうと、関数Initを抜けた時点でデリゲートが消滅し
トラックバーを動かした瞬間に異常終了してしまいます。
（デリゲートがGCで破棄されるまではうまく動いているようにみえますが、いずれ必ず落ちます）



○cvCreateTrackbarについて
トラックバーを定義するcvCreateTrackbarの第三引数では値を格納する領域を指定できます。
これもデリゲート同様にGCによってアクセスできなくなる可能性があるので注意が必要です。
グローバルな位置に定義した変数を指定すれば問題は起き難いですが、
できれば第三引数にはNullを指定し、
コールバック関数の引数かcvGetTrackbarPosでトラックバーの値を取得した方が安全だと思います。
初期値を設定する場合は、cvSetTrackbarPosを使用してください。



○IplImageについて
IplImageにはビットマップを生成する為のメソッドが追加されています。

　UpdateBitmapImageメソッド
　　内部のイメージデータを使ってBitmaoを生成します。
　　対応しているのはビット深度が８ビット(IPL_DEPTH_8U)の
　　チャンネル数が1か3の場合のみです。
　　（チャンネル数が3の場合、Format24bppRgbとして画像を生成します。
　　　チャンネル数が1の場合、グレイスケール画像になります。）

　BitmapImageプロパティ
　　UpdateBitmapImageメソッドにて生成されたビットマップイメージを返します。
　　UpdateBitmapImageが呼び出されない限り更新されません。

　例）
　　Dim img As IplImage = cvCreateImage( cvSize_( 320, 240 ), IPL_DEPTH_8U, 3 )
　　img.UpdateBitmapImage()
　　Picture1.Image = img.BitmapImage
　　Picture1.Refresh

このメソッドを使用することにより
PictureBoxなどにIplImageの画像を表示することができます。
ただし、この方法での描画は決して速くありませんので、
毎フレーム描画するとコマ落ちする原因になります。



○サンプルについて
サンプルプログラムは二つのプロジェクトに分けてあります。

　VBSample
　　OpenCV付属のサンプルを移植したサンプルと、
　　独自に作成したサンプルがあります。

　VBSample2
　　http://opencv.jp/sample にて公開されているサンプルを移植したサンプルです。
　　作成者の怡土順一氏、及び、サイトを支えて頂いている方々に感謝します。
　　VBSample2はOpenCV1.1pre1用のライブラリにのみあります。

