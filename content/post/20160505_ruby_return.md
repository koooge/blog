---
title: "rubyではreturn書かないと思いますが"
date: 2018-03-13T14:29:18.000Z
draft: false
---

前回 [%5Bhttp://www.koooge.com/entry/2016/11/23/143949:embed:cite%5D]
アンケートに参加していただいた方ありがとうございます。
私の不徳の為すところでで母数はなんともですが、47%の人がvoid関数の最後にreturnを書き、残り53%の人が書かないということがわかりました。

まじかよ
まさかのほぼ同数。
ぶっちぎりでreturn書かない派が勝つと思ってました。だってムダじゃないですか・・。
return書く派の方々は何で書くんですかね。理由を聞きたいです。
同僚の同期は「なんとなく」って言ってました。
関数を書く＝returnを書くものみたな感じらしっす

そもそもrubyではreturn書かないよね
javaはおそらくCと変わりないと思いますが、動的型付け言語のRubyなどではどうなのでしょう。
rubyとかは最後に評価された式を返しますので、returnとか書かないを思います

irb(main):001:0>
irb(main):002:0* def func
irb(main):003:1>   value = 1 + 1
irb(main):004:1> end
=> :func
irb(main):005:0> p func
2
=> 2


irb(main):006:0> def func_return
irb(main):007:1>   value = 1 + 1
irb(main):008:1>   return value
irb(main):009:1> end
=> :func_return
irb(main):010:0> p func_return
2
=> 2


irb(main):011:0> def func_value
irb(main):012:1>   value = 1 + 1
irb(main):013:1>   value
irb(main):014:1> end
=> :func_value
irb(main):015:0> p func_value
2
=> 2


どれも結果は同じですが、
ここでわざわざreturn valueと書く方っておそらくいないですよね？多分。。

rubyもstep数をみてみる
rubyの場合はYARVという中間表現でstep数の違いをみてみます。
RubyVM::InstructionSequence.compile(code).disasmを使ってYARVを見ます。
環境は**ruby 2.3.3p222 (2016-11-21 revision 56859) [x86_64-linux]**です。

sdiffの結果を表示します。左がfuncで右がfunc_returnです。

code = <<END                                                    code = <<END
def func                                                      | def func_return
                                                              >   return
end                                                             end
END                                                             END
puts RubyVM::InstructionSequence.compile(code).disasm           puts RubyVM::InstructionSequence.compile(code).disasm


結果

== disasm: #<ISeq:<compiled>@<compiled>>=====================   == disasm: #<ISeq:<compiled>@<compiled>>=====================
0000 trace            1                                         0000 trace            1
0002 putspecialobject 1                                         0002 putspecialobject 1
0004 putobject        :func                                   | 0004 putobject        :func_return
0006 putiseq          func                                    | 0006 putiseq          func_return
0008 opt_send_without_block <callinfo!mid:core#define_method,   0008 opt_send_without_block <callinfo!mid:core#define_method,
0011 leave                                                      0011 leave
== disasm: #<ISeq:func@<compiled>>=========================== | == disasm: #<ISeq:func_return@<compiled>>====================
0000 trace            8                                         0000 trace            8
0002 putnil                                                     0002 putnil
0003 trace            16                                      | 0003 trace            16
0005 leave                                                      0005 leave


むむ、rubyは全く同じになりました。
return書こうが書くまいがパフォーマンスに影響でませんね。
rubyコミッタの方々の頑張りによるものと思いますが、return書かない派の主張が薄れてしまった・・。

まとめ
でもきっとrubyistは無駄にreturn書かないですよね；
