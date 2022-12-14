# Spring JPA๋ฅผ ํ์ฉํ CRUD

## Create
**๐๐ป `Repository`์`save()` ์ด์ฉ**
```java
repository.save(new Post("์ฒ ์์ TIL", "๊น์ฒ ์", "์ค๋ ๋ฐฐ์ด ๊ฒ : REST API"))
```

<br>

## Read
**๐๐ป `Repository`์ `findAll()` ์ด์ฉ**
```java
// ๋ฐ์ดํฐ ์ ๋ถ ์กฐํํ๊ธฐ
List<Post> postList = repository.findAll();
for (int i=0; i<postList.size(); i++) {
    Post post = postList.get(i);
    System.out.println(post.getId());
    System.out.println(post.getTitle());
	System.out.println(post.getAuthor());
   	System.out.println(post.getContent());
}

// ๋ฐ์ดํฐ ํ๋ ์กฐํํ๊ธฐ
Post post = repository.findById(1L).orElseThrow(
        () -> new IllegalArgumentException("ํด๋น ๊ฒ์๊ธ์ด ์กด์ฌํ์ง ์์ต๋๋ค.")
);
```

<br>

## Update
**๐๐ป `Repository`์ `findAll()`๊ณผ `Service`์ `update()` ์ด์ฉ**

```java
@Bean
public CommandLineRunner demo(PostRepository postRepository, PostService postService) {
    return (args) -> {
        postRepository.save(new Post("ํ๋ก ํธ์๋์ ๊ฝ, ๋ฆฌ์กํธ", "์๋ฏผ์"));

        System.out.println("๋ฐ์ดํฐ ์ธ์");
        List<Post> postList = postRepository.findAll();
        for (int i=0; i<postList.size(); i++) {
            Post post = postList.get(i);
            System.out.println(post.getId());
            System.out.println(post.getTitle());
            System.out.println(post.getAuthor());
        }

        Post new_post = new Post("์น๊ฐ๋ฐ์ ๋ด, Spring", "์๋ฏผ์");
        postService.update(1L, new_post);
        postList = postRepository.findAll();
        for (int i=0; i<posstList.size(); i++) {
            Post post = postList.get(i);
            System.out.println(post.getId());
            System.out.println(post.getTitle());
            System.out.println(post.getAuthor());
        }
    };
}
```

> ๐ก **Tip!**<br>
> 
> ์ฌ์ค ์ง๊ธ Update ๊ธฐ๋ฅ๋ง Service๋ฅผ ์ด์ฉํ ๊ฒ ๊ฐ์ ๋ณด์ด์ง๋ง, repository์ ๋ฉ์๋๋ค์ ์ฌ์ฉํ์ง๋ง ์๋ฒ์์ ์ฒ๋ฆฌํ๋ ๋ณต์กํ ์์๋ค์ ์ฌ๋งํ๋ฉด Service์ ๋ฃ์ด์ฃผ๋ ๊ฒ์ด ๊น๋ํ๋ค.<br>
> ์ฆ, ์์์ ์ ๋ฆฌํ ๊ฒ์ฒ๋ผ<br>
> **Controller**์๋ ํ๋ก ํธ์ ์ํตํด์ผ ํ๋ ํ์ ๋ถ๋ถ๋ค๋ง,<br>
> **Service**์๋ ๊ทธ ์ธ,<br>
> **Repository**์๋ DB์ ์ํตํด์ผ ํ๋ ํ์ ๋ถ๋ถ๋ค๋ง ๋ฃ์ด์ฃผ๋ ๊ฒ<br>
> ์ด ๊น๋ํ๊ณ  ๋ณด๊ธฐ ์ข์ ์ฝ๋๋ผ๊ณ  ํ๋ค.

<br>

## Delete
**๐๐ป `Repository`์ `findAll()`๊ณผ `deleteAll()` ์ด์ฉ**

```java
@Bean
public CommandLineRunner demo(PostRepository postRepository, PostService postService) {
    return (args) -> {
        postRepository.save(new Post("ํ๋ก ํธ์๋์ ๊ฝ, ๋ฆฌ์กํธ", "์๋ฏผ์"));

        System.out.println("๋ฐ์ดํฐ ์ธ์");
        List<Post> postList = postRepository.findAll();
        for (int i=0; i<postList.size(); i++) {
            Post post = postList.get(i);
            System.out.println(post.getId());
            System.out.println(post.getTitle());
            System.out.println(post.getTutor());
        }

        Post new_post = new Post("์น๊ฐ๋ฐ์ ๋ด, Spring", "์๋ฏผ์");
        postService.update(1L, new_post);
        postList = postRepository.findAll();
        for (int i=0; i<postList.size(); i++) {
            Post post = postList.get(i);
            System.out.println(post.getId());
            System.out.println(post.getTitle());
            System.out.println(post.getTutor());
        }

        postRepository.deleteAll();
    };
}
```