---
layout: post
category : project
tagline: "Simple Blog by using ActiveRecord and MicroTpl"
tags : [php, blog, ActiveRecord, MicroTpl, dispatch]
---
{% include JB/setup %}

#[Blog](https://github.com/lloydzhou/blog.git)


Simple Blog by using ActiveRecord and MicroTpl.

##install
	git clone https://github.com/lloydzhou/blog.git
    cd blog
    composer install
    php -S 0.0.0.0:9999

## Code

### Step1

include the components and set the db connection(using pdo_sqlite).

    require_once('vendor/autoload.php');
    ActiveRecord::setDb(new PDO('sqlite:blog.db'));

### Step2

define install url to create table and init default user.

```php
    map('GET', '/install', function(){
        ActiveRecord::execute("CREATE TABLE IF NOT EXISTS user (id INTEGER PRIMARY KEY, name TEXT, email TEXT, password TEXT);");
        ActiveRecord::execute("CREATE TABLE IF NOT EXISTS post (id INTEGER PRIMARY KEY, user_id INTEGER, title TEXT,content TEXT, time INTEGER);");
        ActiveRecord::execute("CREATE TABLE IF NOT EXISTS tag (id INTEGER PRIMARY KEY, name TEXT, count INTEGER);");
        ActiveRecord::execute("CREATE TABLE IF NOT EXISTS post_tag (id INTEGER PRIMARY KEY, post_id INTEGER, tag_id INTEGER);");
        $user = new User();
        $user->name = 'admin';
        $user->email = 'admin@example.com';
        $user->password = md5('admin');
        $user->insert();
        redirect('/posts');
    });
```

### Step3

define the models and relations.

    class User extends ActiveRecord{
        public $table = 'user';
        public $relations = array(
            'posts' => array(self::HAS_MANY, 'Post', 'user_id'),
        );  
    }
    class Post2Tag extends ActiveRecord{
        public $table = 'post_tag';
        public $relations = array(
            'post' => array(self::BELONGS_TO, 'Post', 'post_id'),
            'tag' => array(self::BELONGS_TO, 'Tag', 'tag_id'),
        );
    }
    class Tag extends ActiveRecord{
        public $table = 'tag';
    }
  
    class Post extends ActiveRecord{
        public $table = 'post';
        public $relations = array(
            'tags' => array(self::HAS_MANY, 'Post2Tag', 'post_id'),
            'author' => array(self::BELONGS_TO, 'User', 'user_id'),
        );
        public function getTags(){
            return array_map(function($tag){
                return $tag->tag->name;
            }, $this->tags);
        }   
        /* to update the tags, if add tag, need insert record in "tag" and "post_tag" table*/ 
        function updateTag($tags){
            $tags = array_map(function($t){ return trim($t); }, explode(',', $tags));
            foreach($this->tags as $i=>$tag){
                $key = array_search($tag->tag->name, $tags);
                if (false === $key){
                    $tag->tag->count = $tag->tag->count - 1;
                    if ($tag->tag->count > 0)
                        $tag->tag->update();
                    else
                        $tag->tag->delete();
                    $tag->delete();
                } else unset($tags[$key]);//do not change tag
            }
            foreach($tags as $i=>$t){
                $tag = new Tag();
                $post2tag = new Post2Tag();
                $tag->reset()->eq('name', $t)->find();
                if (!$tag->id){
                    $tag->name = $t; 
                    $tag->count = 1;
                    $tag->insert();
                }else{
                    $tag->count = $tag->count + 1;
                    $tag->update();
                }
                $post2tag->tag_id = $tag->id;
                $post2tag->post_id = $this->id;
                $post2tag->insert();
            }
            return $this;
        }   
    }
    
### Step4

define urls to crud the post.

#### Step4.1

define list urls, list all tags and posts. or list post belong some tag.

    map('GET', '/tags', function(){
        MicroTpl::render('list.html', array('tags'=>(new Tag())->orderby('count desc')->findAll()));
    });
    map('GET', '/tag/<id:\d+>/post', function($p){
        $tags = (new Post2Tag())->eq('tag_id', (int)$p['id'])->findAll();
        MicroTpl::render('list.html', array('posts'=>array_map(function($t){ return $t->post; }, $tags)));
    });
    map('GET', '/posts', function(){
        MicroTpl::render('list.html', array('posts'=>(new Post())->orderby('time desc')->findAll()));
    });

#### Step4.2 

define urls to manage the post.

    map('GET', '/post/create', function(){
        MicroTpl::render('post.html', array('user'=>get_user()));
    });
    map('POST', '/post/create', function(){
        $uid = (int)($_POST['user_id']);
        $post = new Post();
        $post->user_id = $uid;
        $post->title = $_POST['title'];
        $post->content = $_POST['content'];
        $post->time = time();
        $post->insert();
        redirect('/post/'. $post->updateTag($_POST['tag'])->id);
    });
    map('GET', '/post/<id:\d+>', function($p){
        MicroTpl::render('post.html', array('post'=>get_post($p['id'])));
    });
    map('GET', '/post/<id:\d+>/delete', function($p){
        $post = get_post($p['id']);
        $post->delete();
        redirect('/posts');
    });
    map('GET', '/post/<id:\d+>/edit', function($p){
        $post = get_post($p['id']);
        MicroTpl::render('post.html', array('user'=>$post->author, 'post'=>$post));
    });
    map('POST', '/post/<id:\d+>/edit', function($p){
        $post = get_post($p['id']);
        $post->title = $_POST['title'];
        $post->content = $_POST['content'];
        $post->update();
        $post->updateTag($_POST['tag']);
        redirect('/post/'. $post->id);
    });

### Step5

enter point.

    dispatch();


