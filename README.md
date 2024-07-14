# LessQL

LessQL is a lightweight and performant alternative to Object-Relational Mapping for PHP.

**[Guide](doc/guide.md) | [Conventions](doc/conventions.md) | [API Reference](doc/api.md) | [About](doc/about.md)**

*If you are looking for an SQL-based approach superior to raw PDO, check out [DOP](https://github.com/morris/dop) as an alternative.*

## Fork Notes

- The notes in this section outlines information regarding differences with the original repository: [morris/lessql](https://github.com/morris/lessql)
- Other sections MAY have ONLY important/appropriate updates made to them compared with the original repository.
- Reasons for fork:
  - Last update is old:
    - (May 10, 2020 at time of writing): [Releases](https://github.com/morris/lessql/releases)
    - [Is this project still maintained?](https://github.com/morris/lessql/issues/39)
    - [Next release...](https://github.com/morris/lessql/issues/29)
  - Errors/warnings when using with newer PHP versions:
    - PHP 8.1 deprecated errors such as with "offsetExists".
    - Row not found exception. require_once of Row fixed it.
  - Still using it in some projects so need to have a working up-to-date source(installed via composer through packagist.).
- Notes:
  - [morris/dop](https://github.com/morris/dop) is mentioned as an alternative, but it does not seem to have much development or be maintained.

## Installation

Install LessQL via composer: `composer require theopenweb/lessql`.
LessQL requires PHP >= 8.0 and PDO.

## Usage

```php
// SCHEMA
// user: id, name
// post: id, title, body, date_published, is_published, user_id
// categorization: category_id, post_id
// category: id, title

// Connection
$pdo = new PDO('sqlite:blog.sqlite3');
$db = new LessQL\Database($pdo);

// Find posts, their authors and categories efficiently:
// Eager loading of references happens automatically.
// This example only needs FOUR queries, one for each table.
$posts = $db->post()
    ->where('is_published', 1)
    ->orderBy('date_published', 'DESC');

foreach ($posts as $post) {
    $author = $post->user()->fetch();

    foreach ($post->categorizationList()->category() as $category) {
        // ...
    }
}

// Saving complex structures is easy
$row = $db->createRow('post', [
    'title' => 'News',
    'body' => 'Yay!',
    'categorizationList' => [
        [
            'category' => ['title' => 'New Category']
        ],
        ['category' => $existingCategoryRow]
    ]
]);

// Creates a post, a new category, two new categorizations
// and connects them all correctly.
$row->save();
```

## Features

- Efficient deep finding through intelligent eager loading
- Constant number of queries, no N+1 problems
- Save complex, nested structures with one method call
- Convention over configuration
- Work closely to your database: LessQL is not an ORM
- No glue code required
- Clean, readable source code
- Fully tested with SQLite3, MySQL and PostgreSQL
- MIT license

Inspired mainly by [NotORM](https://www.notorm.com/),
it was written from scratch to provide a clean API and simplified concepts.

## Contributors

- [theopenwebjp](https://github.com/theopenwebjp)
- [jayaddison](https://github.com/jayaddison)
- [f0ma](https://github.com/f0ma)

Thanks!
