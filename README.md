step by step, 用ruby on rails 创建一个新闻组, 并使用富文本编辑器kindeditor编辑

## 第一章： 建立新闻组的主结构

第一步：确认ruby 版本
在终端输入
```
ruby -v
```
![Snip20170514_1.png](https://ooo.0o0.ooo/2017/05/14/5917e4835ebf8.png)

第二步： 确认rails 版本
在终端输入
```
rails -v
```
![Snip20170514_1.png](https://ooo.0o0.ooo/2017/05/14/5917e567210b4.png)

第三步： 建立一个新的rails 项目
`
rails new company
`
![Snip20170514_3.png](https://ooo.0o0.ooo/2017/05/14/5917ec16ab239.png)

第四步： 进入到刚才新建的项目  ` cd company `

![Snip20170514_4.png](https://ooo.0o0.ooo/2017/05/14/5917eddc0c67e.png)

第五步： 新建一个article的model  `rails g model article title:string content:text `
![Snip20170514_6.png](https://ooo.0o0.ooo/2017/05/14/5917ee853637f.png)

第六步： 数据库迁移 ` rake db:migrete `
![Snip20170514_7.png](https://ooo.0o0.ooo/2017/05/14/5917eefff1080.png)

第七步： 产生articles的controller
![Snip20170514_8.png](https://ooo.0o0.ooo/2017/05/14/5917ef7ee91a1.png)

第八步： 产生一个index的页面。  ` touch app/views/articles/index.html `
然后运行atom编辑器  ` atom . `
![Snip20170514_9.png](https://ooo.0o0.ooo/2017/05/14/5917f771f287e.png)

第九步： 打开atom后， 找到index, 在里面输入   `<h1>Hello, kindeditor</h1>    `
![Snip20170514_12.png](https://ooo.0o0.ooo/2017/05/14/5917fbfd9d086.png)

第十步： 打开config/routes, 在里面输入：  ` root 'articles#index'    `
![Snip20170514_13.png](https://ooo.0o0.ooo/2017/05/14/5917fc9e7b983.png)

第十一步： 在终端的界面， 使用` command + T   `, 新开一个终端
![Snip20170514_14.png](https://ooo.0o0.ooo/2017/05/14/5917fd5208252.png)

第十二步： 进入company的专案   ` cd company  `
![Snip20170514_15.png](https://ooo.0o0.ooo/2017/05/14/5917fdad92de4.png)

第十三步： 运行服务器   ` rails s `
![Snip20170514_16.png](https://ooo.0o0.ooo/2017/05/14/5917fe00c97f4.png)

第十四步： 在浏览器窗口里，输入 ` http://localhost:3000/  ` ， 应该可以看到下面的界面
![Snip20170514_17.png](https://ooo.0o0.ooo/2017/05/14/5917fe9ed5830.png)

## 第二章：建立新闻组的新建、浏览功能

第一步： 建立show,new,create 的controller
打开 articles_controller, 输入：
```
def index
    @articles = Article.all
  end

  def new
    @article = Article.new
  end

  def create
    @article = Article.new(article_params)
  if @article.save
    redirect_to articles_path
  else
    render :new
  end
  end

private
  def article_params
    params.require(:article).permit(:title, :content)
  end

```
![Snip20170514_18.png](https://ooo.0o0.ooo/2017/05/14/591800e6ba6a4.png)

第二步，编辑articles／index 页面
```
<h1>Hello, kindeditor</h1>
<br>
<div>
    <%= link_to("New article", new_article_path) %>
  </div>
<br><br>
<table>
    <thead>
      <tr>
        <td>Title</td>
        <td>content</td>
      </tr>
    </thead>
    <tbody>
      <% @articles.each do |article| %>
        <tr>
          <td><%= link_to(article.title, article_path(article)) %></td>
          <td><%= article.content %></td>
        </tr>
      <% end %>
    </tbody>
  </table>
```
![Snip20170514_29.png](https://ooo.0o0.ooo/2017/05/14/59180ae0aecca.png)

第三步，创建show, new的页面 `touch app/views/articles/new.html.erb`, ` touch app/views/articles/show.html.erb
`
![Snip20170514_20.png](https://ooo.0o0.ooo/2017/05/14/5918047245b7c.png)

第四步， 编辑new.html.erb
```
<div>
    <h2>新增文章add new article</h2>

    <hr>
    <%= form_for @article do |f| %>

    标题title
    <br>
    <%= f.text_field :title %>
    <br>
    内容content
    <br>
    <%= f.text_area :content %>
    <br>

    <%= f.submit "Submit", :disable_with => 'Submiting...' %>
    <% end %>

</div>
```

![Snip20170514_22.png](https://ooo.0o0.ooo/2017/05/14/591805fedfe7f.png)

第五步， 编辑show.html.erb
```
<div>
  <h2><%= @article.title %></h2>
  <p><%= @article.content %></p>
</div>
```

![Snip20170514_23.png](https://ooo.0o0.ooo/2017/05/14/591806a8b080c.png)

第六步， 编辑show的controller
```
  def show
    @article = Article.find(params[:id])
  end
```
![Snip20170514_24.png](https://ooo.0o0.ooo/2017/05/14/591807187844f.png)

第七步， 在routes 里面加入 articles的resources
```
resources :articles
```
![Snip20170514_26.png](https://ooo.0o0.ooo/2017/05/14/591807e0d39c2.png)

第八步，在浏览器中打开 `localhost:3000`, 点击“new article”， 就可以新增文章喽。 增加一篇文章试一试。新增完了，别忘了单击“submit”提交。
![Snip20170514_31.png](https://ooo.0o0.ooo/2017/05/14/59180badb28af.png)
![Snip20170514_27.png](https://ooo.0o0.ooo/2017/05/14/59180bf4a2e37.png)

## 第三章， 加载 kindeditor 编辑器
（源gem的使用文档：https://github.com/Macrow/rails_kindeditor  ）
第一步： 把gem 放到 gemfile
![Snip20170514_32.png](https://ooo.0o0.ooo/2017/05/14/59180d04983b0.png)
第二步： 运行 ` bundle install`
![Snip20170514_33.png](https://ooo.0o0.ooo/2017/05/14/59180d487005c.png)
第三步，安装kindeditor，  ‘rails g rails_kindeditor:install’
![Snip20170514_34.png](https://ooo.0o0.ooo/2017/05/14/59180dd995b6a.png)
第四步， 运行 `rails kindeditor:assets`
![Snip20170514_35.png](https://ooo.0o0.ooo/2017/05/14/59180e864b9e6.png)
第五步， 我们的目的是，在新闻的内容中使用kindeditor编辑器。 修改articles/index.html
```
<%= f.kindeditor :content %>
```
![Snip20170514_36.png](https://ooo.0o0.ooo/2017/05/14/59180f330cfd9.png)

第六步， 打开application/js, 加入 `//= require kindeditor/kindeditor.js`
![Snip20170514_39.png](https://ooo.0o0.ooo/2017/05/14/591812e3d075c.png)

第七步： 重启服务器。 在服务器的界面， 先按一下 `command+c`，然后输入`rails s`
![Snip20170514_41.png](https://ooo.0o0.ooo/2017/05/14/591813872fd1d.png)

第八步， 回到浏览器，`localhost:3000`, 点击“new article”， 新增文章
![Snip20170514_31.png](https://ooo.0o0.ooo/2017/05/14/59180badb28af.png)

第九步， 新增文章界面，点击一下刷新，就可以出现kindeditor编辑器。 我们可以插入链接，改变字体等等编辑功能。新增完毕后点击“submit” 按钮提交
![Snip20170514_43.png](https://ooo.0o0.ooo/2017/05/14/59181448672d3.png)
![Snip20170514_45.png](https://ooo.0o0.ooo/2017/05/14/59181506de644.png)

第十步，在show的页面修改一段代码    `<p><%= raw @article.content %></p>`
在index的页面修改一段代码  `<td><%= raw article.content %></td>`
![Snip20170514_46.png](https://ooo.0o0.ooo/2017/05/14/591815aee9355.png)
![Snip20170514_47.png](https://ooo.0o0.ooo/2017/05/14/59181646c0fb0.png)

第十一步， 完成。 可以看到增加编辑器后的效果。
![Snip20170514_48.png](https://ooo.0o0.ooo/2017/05/14/591816aac5b9b.png)
![Snip20170514_49.png](https://ooo.0o0.ooo/2017/05/14/591816b9c4907.png)
