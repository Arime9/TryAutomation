1

fastlane/[scan](https://docs.fastlane.tools/actions/scan/)を実行する際に必要になるので、Xcode
 Projectを作成する際は`Include Tests`を選択します。

既にProject作成済みの場合は、Targetの`Unit Testing Bundle`と`UI Testing Bundle`を追加します。  
これらはXCTestとXCUITestに該当します。

![](./.assets/xcode_new_project.jpg)
![](./.assets/xcode_project_add_target_test.jpg)

2 fastlaneの動作環境を整えるために各種ソフトウェアをインストールします。

[Setup - fastlane docs](https://docs.fastlane.tools/getting-started/ios/setup/) を参考にご自身で環境構築しても良いですが、この記事ではPCの環境を極力汚さない方法を選びながら説明します。

2-1

[Homebrew](https://brew.sh/index_ja) のインストールがまだの場合は、まずこのスクリプトをターミナルで実行します。

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

2-2

本来は後述の手順を踏んで各種ソフトウェアをインストールする必要があるのですが、この記事では [fast-ruby-install](https://github.com/Tea-and-Coffee/fast-ruby-install) のスクリプトを用いてまとめてインストールと環境構築を行います。

**(本来の大まかなインストール手順)**

- Homebrewで、[rbenv](https://github.com/rbenv/rbenv)と[rbenv/ruby-build](https://github.com/rbenv/ruby-build)をインストールする。  
- rbenvで、[ruby](https://github.com/ruby/ruby)及び[rubygems](https://github.com/rubygems/rubygems)をインストールする。  
- rubygemsで、[bundler - rubygems](https://github.com/rubygems/rubygems/tree/master/bundler)をインストールする。

`-ruby 2.7.3` `-gem 3.2.28` `-bundlers 2.2.28` にインストールしたいソフトウェアバージョンを指定して、ターミナルで実行します。  
この記事ではrubyは2.7の最新バージョンの2.7.3、rubygemsとbundlerは最新バージョンの3.2.28と2.2.28に指定しています。

```bash
curl -fsSL https://raw.githubusercontent.com/Tea-and-Coffee/fast-ruby-install/master/install.sh | bash -s -- --ruby 2.7.3 --gem 3.2.28 --bundlers 2.2.28
```

2-3

続いてbundlerの設定ファイルを作成します。  
実行後に`Gemfile`というテキストファイルが作成されます。

```bash
bundle init
```

2-4

`Gemfile`をテキストとして開き、後述を参考に編集します
この記事では現状の最終バージョンを指定しています。 詳細なバージョン指定方法については知りたい場合は [Bundler: gemfile](https://bundler.io/man/gemfile.5.html) を参照して下さい。

```
source "https://rubygems.org"

gem "fastlane", "~> 2.195.0"
```

[CocoaPods](https://github.com/CocoaPods/CocoaPods)や[xcpretty/xcode-install](https://github.com/xcpretty/xcode-install)もインストールする場合はこの様になります。

```
source "https://rubygems.org"

gem "fastlane", "~> 2.195.0"
gem "cocoapods", "~> 1.11.2"
gem "xcode-install", "~> 2.8"
```

2-5

`Gemfile`の編集が済んだら、Gemfileに記載のソフトウェアをインストールするために実行します。

```bash
bundle install
```

`vendor/bundle`配下にgems（fastlaneやその依存関係のあるソフトウェア）がインストールされますので、必要に応じて`.gitignore`に`vendor/`を追記します。
