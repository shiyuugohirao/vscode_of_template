# vscode_of_template

VSCode+WinでopenFrameworksのビルド+実行をするための設定ファイルです。msys2を使ってやるのは色々大変なので、VisualStudioのソリューションファイルにビルドコマンドを投げてビルドしてもらうという方式になっています。oFはVisualStudio版をダウンロードしてください。

## ビルドするまで

- ProjectGeneratorでVisualStudio用のプロジェクトを作成する
- リポジトリの.vscode一式を、プロジェクトフォルダ(.slnや.vcxprojがある階層)に入れる

- `Ctrl + Shift + B` で isDefault(ReleaseBuild_x64)を実行
- `Ctrl + Shift + P` > [Run Tasks] で任意のBuildTaskを実行

## 各種ファイルの設定

### launch.json と tasks.json

- ~~`$$$YOURPROJECTNAME$$$` をそれぞれ自分のプロジェクト名に変更する~~
    - 基本的には `${workspaceFolderBasename}` がプロジェクト名になるはずなのでYOURPROJECTNAMEを置き換えました。

### c_cpp_properties.json

- ~~compilerPathにあるVisual Studioのパスがバージョンで違ったりするので、自分のPCに入っているパスに合わせる~~
    -  VisualStudioをインストールしている場合は、直接指定しなくても大丈夫そうでした。
- addonsがある場合は、includePathにアドオンのパスを追加すると補完もしてくれる
- F5でビルド＆実行


### msbuild

- VisualStudioのmsbuildコマンドを利用してビルドを行うためmsbuildへのパスを通しておく必要があります。  
環境パスに `C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Current\Bin` など MSBuild.exeのあるパスを追加してください。  
※環境パスへ追加後でもVSCodeのTerminalで認識できない場合があるようです。自分の場合、一度PCを再起動すると認識しました。

### include

- includeで赤線のエラーが出る場合、c_cpp_properties.jsonの指定がうまくできていない可能性が高いです。
- includeまわりでエラーが出ていても一応、定義にジャンプしたり、ビルドできる場合もあります。
- VscodeのC_Cpp:IntelliSenseEngineの設定でTagParserを設定している場合、includeのエラーは出ませんが、合わせてそのほかのエラーも表示されなくなるようです。  
IntelliSenseEngineはDefaultなどにし、c_cpp_properties.jsonにinclude用のパスを指定するのが良さそうです。


### VisualStudio2019

- ProjectGeneraterからVisualStudio用のプロジェクトを生成すると通常VS2017用のプロジェクトが生成されます。  
このためVS2019を使用する場合(VS2019のmsbuildを利用する場合)、ソリューションの再ターゲットが必要になります。以下のいずれかの対応を行ってください。
    -  一度VisualStudio2019でプロジェクトを起動し、ソリューションの再ターゲットを行う。
    - YOURPROJECT.vcxproj に記述されている `<PlatformToolset>v141</PlatformToolset>`を`<PlatformToolset>v142</PlatformToolset>` に変更する。
        - 参考 [MSBuild プロジェクト ファイルを作成するには (4)](https://docs.microsoft.com/ja-jp/cpp/build/walkthrough-using-msbuild-to-create-a-visual-cpp-project?view=msvc-160#to-create-the-msbuild-project-file)
