# QuarkusでRESTAPIを構築する（MacOS）

## 環境構築（Java）

以下をインストールする

- Java 11+
- Maven 3.6.2+
- GraalVM（ネイティブイメージを作成する）

※バイナリを実行すると、Appleのセキュリティでブロックされる場合がある

システム環境設定 > プライバシーとセキュリティ > ブロックされたバイナリを許可 で実行できるようになる

### Java

Macなら`homebrew`でインストールできる

```
brew install openjdk
```

### Maven

1. homebrewでインストールする

```
brew install maven
```

2. コマンドが使えればOK
```
mvn -v
```

### GraalVM（オプション）

1. [GraalVM公式](https://www.graalvm.org/downloads/)から環境に合ったものをダウンロードする

2. GraalVMを解凍して、指定の場所に移動する

```
sudo mv path/to/your/graalvm-folder /Library/Java/JavaVirtualMachines/
```

3. jenvをインストールする

```
brew install jenv
```

4. シェルセッション開始でjenvを初期化するようにする

`~/.bash_profile`や`~/.bashrc`等に下記を追記する（環境による）

`JAVA_HOME`を設定することで、`maven`がjenvで設定したjavaのバージョンを参照させる

```.bashrc
# jenvを初期化する
if which jenv > /dev/null; then eval "$(jenv init -)"; fi
export JAVA_HOME=$(jenv prefix)
```

5. 設定を読み込む

~/.bash_profile, ~/.bashrc, ~/.zshrc, 等

```
source ~/.bashrc
```

6. jenvにjavaのバージョンを追加する

```
jenv add /path/to/graalvm-folder/Contents/Home
jenv add /path/to/other-java-folder/Contents/Home
```

7. jenvでjavaのバージョンを切り替える

```
jenv versions # 使用できるバージョン一覧を確認する
jenv global 19.0.1 # デフォルトのjavaに切り替える
jenv global graalvm-version # GraalVMのバージョンに切り替える
```

8. キャッシュを更新する

```
jenv rehash
```

9. javaのバージョンを確認する

```
java -version
```

## Quarkusプロジェクトのスタート

基本的には、[Quarkus公式サイト](https://ja.quarkus.io/)の手順に従う。

### インストール

JBangを使用してQuarkus CLIをインストールする

```
curl -Ls https://sh.jbang.dev | bash -s - trust add https://repo1.maven.org/maven2/io/quarkus/quarkus-cli/
curl -Ls https://sh.jbang.dev | bash -s - app install --fresh --force quarkus@quarkusio
```

### Quarkusアプリケーションを作成する

```
quarkus create app quarkus-app
cd quarkus-app
```

3. アプリを実行する

```
quarkus dev
```

4. [localhost:8080](http://localhost:8080)にアクセス

5. RESTエンドポイントにアクセス

`Hello from RESTEasy Reactive`が出れば成功

```
curl localhost:8080/hello
```

### テスト

```
./mvnw test
```

### 依存パッケージのインストール

Quarkusは必要に応じて拡張機能を追加できる

- JPA(Java Persistence API)とPostgreSQLをインストール

```
quarkus add extension --extensions="hibernate-orm-panache, jdbc-postgresql"
```
