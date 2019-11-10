あとで消す (ここから): 

- test/stdlib/Thread_test.rb　を追加しようと yak shaving しまくって失敗したブランチ
- https://github.com/ruby/ruby-signature/pull/67/files を見ると、少量のテストで型検査を発動するのが正しそう
- 間違った内容
- Ruby 2.7 の [test/ruby/test_thread.rb](https://github.com/ruby/ruby/blob/master/test/ruby/test_thread.rb) を test/stdlib/Thread_test.rb として使おうとしたのが判断間違いだった模様 orz
- なので、 [tool/lib/test/unit/core_assertions.rb](https://github.com/ruby/ruby/blob/master/tool/lib/test/unit/core_assertions.rb) などを Ruby 2.7 ソースツリーからコピーしまくった。
- 結局 `bundle exec ruby bin/test_runner.rb Thread` の結果は `87 runs, 265 assertions, 0 failures, 2 errors, 0 skips`
- `assert_raise` がないとか、 `test/unit` が読み込めないとかの基本的なエラーはなくなったけど、目的・目標を見誤り失敗した。
- 型検査が動く少量でシンプルなテストだけを書くのが作法にかなっていそうな感じ。
- もっと空気を読んでから作業をするべきだった orz

あとで消す (ここまで):


# Ruby::Signature

Ruby::Signature provides syntax and semantics definition for `the` Ruby Signature language, `.rbs` files.
It consists of a parser, the syntax, and class definition interpreter, the semantics.

![Travis master](https://travis-ci.com/ruby/ruby-signature.svg?branch=master)

## Build

We haven't published a gem yet.
You need to install the dependencies, and build its parser.

```
$ bundle
$ bundle exec rake parser
$ bundle exec exe/ruby-signature
```

## Usage

```
$ ruby-signature list
$ ruby-signature ancestors ::Object
$ ruby-signature methods ::Object
$ ruby-signature method ::Object tap
```

### ruby-signature [--class|--module|interface] list

```
$ ruby-signature list
```

This command lists all of the classes/modules/interfaes defined in `.rbs` files.

### ruby-signature ancestors [--singleton|--instance] CLASS

```
$ ruby-signature ancestors Array                    # ([].class.ancestors)
$ ruby-signature ancestors --singleton Array        # (Array.class.ancestors)
```

This command prints the _ancestors_ of the class.
The name of the command is borrowed from `Class#ancestors`, but the semantics is a bit different.
The `ancestors` command is more precise (I believe).

### ruby-signature methods [--singleton|--instance] CLASS

```
$ ruby-signature methods ::Integer                  # 1.methods
$ ruby-signature methods --singleton ::Object       # Object.methods
```

This command prints all methods provided for the class.

### ruby-signature method [--singleton|--instance] CLASS METHOD

```
$ ruby-signature method ::Integer '+'               # 1+2
$ ruby-signature method --singleton ::Object tap    # Object.tap { ... }
```

This command prints type and properties of the method.

### Options

It accepts two global options, `-r` and `-I`.

`-r` is for libraries. You can specify the names of libraries.

```
$ ruby-signature -r set list
```

`-I` is for application signatures. You can specify the name of directory.

```
$ ruby-signature -I sig list
```

## Guides

- [Writing signatures guide](doc/sigs.md)
- [Stdlib signatures guide](doc/stdlib.md)
- [Syntax](doc/syntax.md)

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake test` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/ruby/ruby-signature.
