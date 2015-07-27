---
layout: post
title: '“Ready Player One” References'
lang: en
style: |
  .search { padding: 0.5em 1em; width: 100%; }
---

Do you often find yourself in a situation where you don't know what to read,
watch or listen to? I do. Or at least I did, until I read "Ready Player One"
by Ernest Cline. The book is packed with references and allusions to novels,
authors, films, directors, bands and songs for folks raised in the 70s, 80s
or 90s (and with a slight geeky touch maybe).

Due to "Ready Player One" being a novel (and one of the best science fiction
novels in the last years!), there is quite an amount of text around the actual
references. I made the effort to extract every reference and allusion I could
find and compiled the following list. The page number refers to the first page
(in the first edition of the book) where I have found the reference.

If you find something wrong or missing, you can contribute by creating a Git
pull request for your changes to the
[data file](https://github.com/towbi/towbi.github.io/blob/master/_data/ready-player-one-references.json)
that is used to generate the list below. If you don't understand what I just
wrote, you can also just [email me](mailto:tn@movb.de) your changes :-). Now
enjoy!

<div id="rporefs">
  <input class="search" placeholder="Search for reference or tag" />

  <table>
    <thead>
      <tr>
        <th>Page</th>
        <th>Reference</th>
        <th>Tags</th>
        <th>Comment</th>
      </tr>
    </thead>
    <tbody class="list">
      {% for ref in site.data.ready-player-one-references %}
        <tr>
          <td class="page">{{ ref.page }}</td>
          <td class="reference"><strong>{{ ref.reference }}</strong></td>
          <td class="tags">{{ ref.tags }}</td>
          <td class="comment">{{ ref.comment }}</td>
        </tr>
      {% endfor %}
    </tbody>
  </table>
</div>

## Other existing lists

I began this list without checking the internet for existing lists (I was
offline on vacation). When I finally did, I found a impressively thorough
list on [shmoop](http://www.shmoop.com/ready-player-one/allusions.html).
That list, however, contains some types of pop cultural references like
clothing, food, etc. and thus is outside the scope of the list I intended.
The shmoop list has additional links, though, which is pretty helpful.

There are at least two lists of movies referenced in the book, one is
[on Letterboxd](http://letterboxd.com/bbbgtoby/list/every-movie-referenced-in-ready-player-one/)
and one is
[on IMDb](http://www.imdb.com/list/ls075756472/).

The _soundtrack to the book_ has been compiled over at
[Castle Anorak](http://castleanorak.com/ready-player-one-soundtrack/).

## Improvements

* Links to IMDb, Wikipedia, etc. for each item
* Page numbers are OK, but an additional reference using the
  _chapter.paragraph_ notation the shmoop list uses would be great too

<script src="/js/list.min.js"></script>

<script>
  var options = {
    valueNames: [ 'reference', 'tags', 'comment' ],
    page: 1000
  };
  var rpoReferences = new List('rporefs', options);
</script>
