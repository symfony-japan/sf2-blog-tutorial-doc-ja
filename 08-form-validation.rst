blogチュートリアル(8) データのバリデーション
============================================

.. note::

    この記事は、Symfony2 BETA2およびBETA3バージョンで動作確認しています。Symfony2がバージョンアップすると、動作しなくなる恐れがあります。

最近のブラウザを使っている人は、
前のステップでデータを追加するフォームのタイトルや本文に何も入力しないで送信しようとすると、
警告がでてくるため、バリデーションが聞いているものと思ったことでしょう。
Formオブジェクトが自動的に HTML5 の required 属性を出力したためです。

HTML5 を解釈しないブラウザでは、データを追加するフォームのタイトルや本文に何も入力しないで送信すると、
エラー画面が表示されることでしょう。

今回は、このフォームにバリデーションルールを設定して、正しくデータが登録できるようにします。

バリデータの設定
----------------

フォームのバリデーションを有効にするには、YAMLやXMLで設定ファイルを記述するか、
アノテーションやphpコードでモデルに直接記述します。
以前のステップでORMの設定をアノテーションで記述したので、
バリデーションもアノテーションで記述します。

\ ``src/My/BlogBundle/Entiry/Post.php``\ を開いて、\ ``$title``\ と\ ``$body``\ の変数定義箇所を
以下のように変更します。

.. code-block:: php

    // src/My/BlogBundle/Entiry/Post.php
    use Symfony\Component\Validator\Constraints as Assert;
    
    // ...
    
        /**
         * @ORM\Column(type="string", length=255)
         * @Assert\NotBlank()
         * @Assert\MinLength(2)
         * @Assert\MaxLength(50)
         */
        protected $title;
    
        /**
         * @ORM\Column(type="text")
         * @Assert\NotBlank()
         * @Assert\MinLength(10)
         */
        protected $body;
    
    // ...

\ ``Symfony\Component\Validator\Constraints``\ 名前空間を\ ``Assert``\ という別名で読み込み、
各バリデーションルールを記述しています。\ ``NotBlank``\ や\ ``MinLength``\ など、
バリデーションの意味は文字通りです。

バリデーションをアノテーションで定義する場合は、もう1つ設定が必要になります。
\ ``app/config/config.yml``\ を開いて、\ ``framework.validation``\ の設定値を以下のように変更します。

.. code-block:: yaml

    # app/config/config.yml
    framework:
        # ...
        validation:      { enabled: true, enable_annotations: true }
        # ...

ブラウザで確認
--------------

再び、記事の追加画面をブラウザで確認してみてください。
今度は、どちらのフォームにも1文字ずつ入れて送信すると、
英語のエラーメッセージが表示されるようになったはずです。
