---
title: rspecで詰まった話
slug: rspec-rails
pubDate: '2024-01-02'
image: /assets/rails.png
category:
  - rails
description: rspecテストでの名前衝突による不具合を解決した話です
---

### 環境
いいね機能のRSpecのrequestスペックで

```ruby
require "rails_helper"

RSpec.describe "Api::V1::Current::Likes", type: :request do
  let(:user) { create(:user) }
  let(:post) { create(:post, user:) }
  let(:headers) { user.create_new_auth_token }

  describe "POST /api/v1/current/posts/:post_id/likes" do
    subject { post api_v1_current_post_likes_path(post_id: post.id), headers: }

    it "creates a new like for the post" do
      expect { subject }.to change { Like.count }.by(1)
      expect(response).to have_http_status(:created)
    end
  end

  describe "DELETE /api/v1/current/likes/:id" do
    subject { delete api_v1_current_like_path(id: like.id), headers: }

    let!(:like) { create(:like, user:, post: post) }

    it "deletes the like" do
      expect { subject }.to change { Like.count }.by(-1)
      expect(response).to have_http_status(:no_content)
    end
  end
end
```
と書き、テストを実行したところ。

```bash
1) Api::V1::Current::Likes POST /api/v1/current/posts/:post_id/likes creates a new like for the post
     Failure/Error: subject { post api_v1_current_post_likes_path(post_id: post.id), headers: headers }

     ArgumentError:
       wrong number of arguments (given 2, expected 0)
     # ./spec/requests/api/v1/current/likes_spec.rb:9:in
```
とエラーになってしまった。
どうやら「post」が名前衝突しているためにhttpのPOSTリクエストが通らなくなっているらしい。

原因は
let(:post) { create(:post, user:) }
と定義しているためで。
let(:test_post) { create(:post, user:) }
と名前を変更すると上手くいった。

という初心者のミスでした。
そもそも、postというテーブルを作ったのがよくなかった（articleとかがよかった？）



