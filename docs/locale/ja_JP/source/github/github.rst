**GitHub Contributions**
========================

リポジトリをフォーク
-------------------

Hyperledger Fabricのソースコードを保護し、
公式GitHubリポジトリのクリーンな状態を維持するため、
Hyperledger FabricのGitHubプルリクエスト (PR) は
フォークされたリポジトリからのみ受け付けています。
GitHubリポジトリをフォークすると、
GitHubの個人用アカウントにリポジトリの同一コピーが作成されます。
その後、コードを編集し、GitHubのプルリクエストプロセスを通じて、
フォークしたHyperledger Fabric公式リポジトリに変更を提案することができます。

リポジトリをフォークするためには:

- ブラウザでフォークしたいGitHubリポジトリに移動します
- 右上のForkボタンを選択します

.. image:: ../images/fork.png
   :scale: 50%

- フォークが完了すると、ブラウザが自動的にGitHubの個人用アカウント内のフォークしたリポジトリに移動します。

これで、個人用フォークをローカルマシンにクローンすることができます。

リポジトリのクローンとUpstreamプロジェクトとの同期
-----------------------------------------------

リポジトリをフォークしたら、ローカルマシンにプロジェクトをクローンして開発作業を開始できます。
これにより、ローカルマシン上にローカルGitHubリポジトリが作成されます。

.. Note ::

   前提条件: このガイドでは、GitHubのSSHプロトコルを使用してリポジトリをクローンします。
   GitHubへのSSHアクセスを設定していない場合は、
   `GitHubのガイド <https://help.github.com/en/articles/connecting-to-github-with-ssh>`_
   を参照して、SSHアクセスを設定してください。

リポジトリをクローンするためには:

- ターミナルを開きます
- リポジトリをクローンしたいローカルディスクの場所に移動します

.. note::
   Goモジュールを使用していないGoベースのリポジトリの場合、
   ディスク上の場所はGOPATHの `src` ディレクトリに対する相対パスでなければなりません。
   つまり、 `$GOPATH/src/github.com/hyperledger` です。

- E次のコマンドを実行して、あなたのフォークをクローンします

.. code::

   git clone git@github.com:<your_github_id>/<repository_name>.git

- 次に、リポジトリのディレクトリに移動し、ローカルリポジトリをリモートのUpstreamリポジトリと同期します

.. code::

   cd <repository_name>
   git remote add upstream https://github.com/hyperledger/<repository_name>.git

- リモートブランチをリストアップし、ローカルリポジトリがリモートのUpstreamリポジトリとのリンクを作成したことを確認できます

.. code::

   git remote -v

これで、フォークしたリポジトリをクローンし、そのUpstreamリポジトリを設定しました。
あなたは開発を開始できます。

開発作業用にローカルフィーチャーブランチを作成
-------------------------------------------

フォークしたリポジトリ内の既存のブランチの状態を保護し、
あなたが行った作業が論理的な場所に保存されるようにするには、
フォークしたリポジトリにおいてフィーチャーブランチを使用することをお勧めします。
フィーチャーブランチは既存のブランチから作成され、GitHubリポジトリのフォークに変更をプッシュする前に、
開発作業を行う場所となります。フィーチャーブランチを作成するには、以下の手順を実行します:

- Upstreamリポジトリからプロジェクトブランチを取得します

.. code::

   git fetch upstream

- 既存のブランチのひとつをチェックアウトします

.. code::

   git checkout -t origin/main

- Upstream側の対応物をローカルのmainにマージします

.. code::

   git merge upstream/main

- GitHub上のフォークをUpstreamのmainから変更があれば更新します

.. code::

   git push origin main

- これであなたは新しいローカルフィーチャーブランチをチェックアウトできます。
  これにより、ローカルのmainブランチがリモートブランチから乖離しないようにできます。
  このフィーチャーブランチは、作成元のブランチとまったく同じコピーとなります。

.. code::

   git checkout -b <feature_branch_name>

これにて、ローカルフィーチャーブランチを作成したので、更新を行うことができます。

変更のコミットとフォークしたリポジトリへのプッシュ
----------------------------------------------

ローカルのフィーチャーブランチで行う作業が完了したら、
そのコードをコミットし、フォークしたリポジトリにプッシュして状態を保存します。
これは、Hyperledgerリポジトリに対するプルリクエストを行うための前提条件です。
次の手順を実行して、コードをコミットし、フォークしたリポジトリにプッシュします:

- 以下のコマンドを実行して、コミットに変更した既存のファイルを追加します。
  '-p'フラグを指定すると、コミットに追加する前に変更内容を確認し、承認するための対話型ターミナルが開きます:

.. code::

   git add -p

- 作成した新しいファイルを追加するには、次のコマンドを実行します:

.. code::

   git add <file1> <file2>

- 追加した変更を含むコミットを作成できます。コミットメッセージには、以下の情報を含める必要があります:

  - タイトルとして、このコミットでの作業の1行の要約を記載し、その後に空行を挿入します。
  - コミットメッセージ本文では、この変更が必要な理由と、そのアプローチについて説明します。
    これにより、レビュアはコードをより理解しやすくなり、レビュープロセスが迅速化されることがよくあります。
  - GitHub Issue (存在する場合) へのリンクを、"Resolves #<GitHub issue number>" のような構文を使用して作成します。
    これにより、GitHub Issue が自動的にリンクされ、このPRがマージされた際にクローズされます。
  - (optional) 新しいテストが追加されていない場合には、コードのテスト方法。

.. code::

   git commit -s

.. note::

   Hyperledgerでは、コミットにはコミッタの署名が必要となります。
   `commit` コマンドを発行する際に、 `-s` フラグを指定すると、コミットにあなたの署名が自動的に追加されます。

- これでローカルの変更内容をフォークしたリポジトリにプッシュできます

.. code::

   git push origin <feature_branch_name>

.. note::

   変更をプッシュする前に元のリポジトリからのUpstreamの変更を統合したい場合は、
   このページの下部にある `フォークしたリポジトリをUpstreamのリポジトリと同期させる`_ というタイトルのセクションを参照してください。

これで、ローカルの変更をフォークしたリポジトリに正常にプッシュすることができました。
これらの変更を統合するには、プルリクエストの手続きを行う必要があります。

GitHubでプルリクエストをオープンする
--------------------------------

フォークしたリポジトリ内のフィーチャーブランチを作成し、変更をプッシュしたので、
今度はフォーク元のHyperledgerリポジトリに対してプルリクエストをオープンし、
コードレビュープロセスを開始することができます。

- まず、ブラウザで `https://github.com/hyperledger/<original_repository>` に移動します
- ページ上部の `Pull Requests` タブを選択します
- Pull Requests ページの右上にある `New Pull Request` を選択します
- Compare Changes ページで、ページ上部の `compare across forks` を選択します
- フォークを作成したHyperledgerリポジトリを `base repository` に、マージしたいブランチを `base` に選択します
- あなたのフォークを `head repository` に、フィーチャーブランチを `compare` に選択します

.. image:: ../images/pull_request.png
   :scale: 50%

- `Create Pull Request` を選択します
- プルリクエストのタイトルと、必要に応じてコメントを入力できます
- プルリクエストを作成するには、2つのオプションから1つを選択できます。
  緑色の `Create Pull Request` ボックスで、右側の下向き矢印を選択します。
- 最初のオプションを選択すると、プルリクエストがそのままオープンされます。
  これにより、リポジトリのメンテナがプルリクエストのレビュアとして自動的に割り当てられます。
- 2番目のオプションを選択すると、プルリクエストが下書き (Draft) としてオープンされます。
  プルリクエストをDraftとして作成すると、レビュアは割り当てられませんが、あなたの変更によってCIは実行されます。

おめでとうございます。これでHyperledgerプロジェクトにはじめてプルリクエストを投稿しました。
プルリクエストは現在、CIで処理中です。
プルリクエストの `Checks` タブに移動すると、プルリクエストのCI処理状況を確認できます。

.. warning::

   もしあなたがこの所定のプルリクエストプロセスを回避し、
   GitHubのエディタUIを使用して行った編集からプルリクエストを生成する場合は、
   UIでコミットが生成された際に、コミットメッセージに署名を手動で追加する必要があります。

プルリクエストの更新
-----------------------
プルリクエストにレビューコメントが寄せられた場合、コミットに編集を加える必要が生じる場合があります。
作業中のローカルブランチに、さらにコミットを追加し、上記の手順に従ってプッシュし直します。
これにより、プルリクエストに新しいコミットが自動的に追加され、CIチェックが再実行されます。

しかし、通常はすべての変更履歴を残しておくことは望ましくありません。
プルリクエストとUpstreamへの最終的なマージを「クリーン」な状態に保つには、
コミットを1つの最終コミットにまとめる (squashする) ことができます。
例えば、直近の2つのコミットを1つのコミットにまとめるには、次の手順を実行します:

.. code::

   git rebase -i HEAD~2

これにより、対話型ダイアログが開きます。
ダイアログで、2つ目 (およびそれ以降) のコミットアクションを 'pick' から 'squash' に変更します。
ダイアログにはすべてのコミットメッセージが表示されますので、最終的なメッセージに編集します。

次に、あなたのリモートオリジンに強制プッシュ (force push) します:

.. code::

   git push origin <feature_branch_name> -f

これにより、あなたのリモートオリジンが最終的な単一コミットに更新され、プルリクエストもそれに応じて更新されます。

あるいは、2つ目のコミットを作成してマージするのではなく、元のコミットを修正してリモートオリジンに強制プッシュすることもできます:

.. code::

   git add -p
   git commit --amend
   git push origin <feature_branch_name> -f

この場合も、プルリクエストは適宜更新され、CIチェックが再実行されます。

あなたのPRを他のブランチにチェリーピックする
----------------------------------------

あなたのPRがmainブランチにマージされた後、
それ以前のブランチにバックポートすべきかどうかを検討する必要があります。
内容が次のリリースに向けての新しい機能である場合は、バックポートは明らかに適切ではありません。
しかし、既存のトピックに対する修正や更新である場合は、必要に応じて、そのPRを以前のブランチにチェリーピックすることを忘れないでください。
判断に迷う場合は、そのPRをマージしたメンテナに相談してください。
両当事者はバックポートを考慮すべきであり、どちらの当事者もそれを実行できます。
GitHubのcherry-pickコマンドを使用するか、または、マージされたPRのコメントとして次のコマンドを貼り付けるという簡単な方法もあります:

.. code::

   @Mergifyio backport release-2.0

``2.0`` はバックポート先のブランチに置き換えてください。
マージの競合が発生しなければ、そのブランチに新しいPRが自動的に作成され、通常の承認プロセスを経てマージされます。
バックポートしたい各ブランチの元のPRにコメントを追加することを忘れないでください。

マージの競合が発生した場合は、代わりにGitHubの ``cherry-pick`` コマンドを使用し、mainブランチのコミットの ``SHA`` を指定します。

- 以下の例では、mainブランチからrelease-2.0ブランチにコミットをチェリーピックする方法を示します:

.. code::

  git checkout release-2.0

- ブランチが遅れている場合は、以下のコマンドを実行して最新の変更を取り込み、ローカルブランチにプッシュします:

.. code::

  git pull upstream release-2.0
  git push origin release-2.0

- 新しいローカルブランチを作成してコンテンツをチェリーピックし、mainブランチのSHAを指定してコンテンツをチェリーピックします:

.. code::

  git checkout -b <my2.0branch>
  git cherry-pick <SHA from main branch>

- マージの競合を解決し、ローカルブランチにプッシュします:

.. code::

  git push origin <my2.0branch>

- 次に、ブラウザでローカルブランチからrelease-2.0ブランチへのPRを作成します:

あなたの変更内容はrelease-2.0ブランチに選択的にバックポートされたので、あとは通常のプロセスに従って承認およびマージすることができます。

ローカルおよびリモートフィーチャーブランチのクリーンアップ
---------------------------------------------

機能ブランチでの作業が完了し、変更がマージされたら、ローカルおよびリモートのフィーチャーブランチは削除してください。
これらは、ビルドにはもはや有効ではないからです。次のコマンドを実行して削除できます:

.. code::

   git branch -d <feature_branch_name>
   git push --delete origin <feature_branch_name>

フォークしたリポジトリをUpstreamのリポジトリと同期させる
----------------------------------------------

開発が進むにつれ、常に新しいコミットが元のプロジェクトにマージされます。
予期せぬマージの競合を避けるため、これらの変更をローカルリポジトリに統合する必要があります。
Upstreamのリポジトリからの変更を統合するには、mainブランチの変更に取り組んでいると仮定して、
リポジトリのルートから次のコマンドを実行します:

.. code::

   git fetch upstream
   git rebase upstream/main

フォークの同期では、ローカルのリポジトリのみが更新されます。
これらの更新を保存するには、次のコマンドを使用して、フォークしたリポジトリにプッシュする必要があります:

.. code::

   git push origin main
