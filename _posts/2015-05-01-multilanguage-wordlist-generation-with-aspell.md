---
layout: post
title: Multilingual ASCII-wordlist generatio with aspell
---

This shit is tight.

Foo 1 | Foo 2
------|------
moin  | :-)

{% highlight perl %}
sub unicount {
    my $pointy = shift;

    if (ref($pointy) eq 'REF' and ref($$pointy) eq 'Set::Tiny') {
        return $$pointy->size();
    }
    else {
        # advance pointy to next element in tree
        my $c = 0;
        if (ref($pointy) eq 'HASH') {
            for my $key (keys %$pointy) {
                $c += unicount($pointy->{$key});
            }
        }
        else {
            warn "What is a " . ref($pointy) ."!?\n";
            print STDERR Dumper($pointy);
        }
        return $c;
    }
}
{% endhighlight %}
