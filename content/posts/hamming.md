---
title: Hamming exercise
date: 2016-08-16T20:21:43+01:00
---

When you try to become web developer there are obvious things you need to learn. HTML is the foundation of each website, so you learn that. Then you start to think that it would be nice if the website looked just a bit better and you learn CSS. Then you figure out you need some JS to make it more dynamic and some backend framework and in my case you start to learn Ruby on Rails. But, there is also one more important part, which shouldn't be neglected if you aspire to become good programmer someday. We should learn how to write clean code and in the case of using objective oriented language how to write good OOP code.

[Arkency](http://blog.arkency.com) knows that and they also live by DDD. When I talked to [Andrzej Krzywda](https://twitter.com/andrzejkrzywda) he explained that learning how to write nice, concise, idiomatic Ruby code will also be part of my course. And today’s blog post will be about my first tries to do that. Most of the tasks that I will be doing in the part can be found on [Exercism.io](http://exercism.io/). First, two tasks were fairly simple and I will not bother you with them. But the next one called Hamming exercise was where the real fun began. In that exercise, I was asked to create a method that returns a number of mutations between two DNA strands. For example, for DNAs “AGA” and “AGG” it should return 1. If you want to read more check out the [Readme](http://exercism.io/exercises/ruby/hamming/readme).

My first solution passed all the tests and used some methods that you can find in the Enumerable module. I think using those methods when writing Ruby makes code easier to read.

```language-ruby
  class Hamming
    def self.compute(original_strand, copied_strand)
      raise ArgumentError if original_strand.length != copied_strand.length
      original_strand = original_strand.split(//)
      copied_strand = copied_strand.split(//)

      original_strand.keep_if.with_index{ |x, i| x != copied_strand[i]}.count
    end
  end
```

Arkency members checked it and Andrzej commented on it telling me that this solution looks ok if we treat DNA strands as simple strings and nothing more. And that my solution doesn't have any information about the domain. After couple of iterations I have created solution that looks like this:
```language-ruby
class Hamming
  def self.compute(original_strand, copied_strand)
    raise ArgumentError if original_strand.length != copied_strand.length
    original_strand = DNA::Strand.new(original_strand)
    copied_strand = DNA::Strand.new(copied_strand)

    original_strand.mutations(copied_strand).count
  end
end

module DNA
  class Strand

    def initialize(strand)
      @strand = strand
    end

    def nucleoids
      @strand.split(//).map{ |nucleoid| Nucleoid.new(nucleoid) }
    end

    def mutations(copied_strand)
      nucleoids.keep_if.with_index{ |x, i| x.nucleoid != copied_strand.nucleoids[i].nucleoid }
    end
  end

  class Nucleoid
    attr_reader :nucleoid

    def initialize(nucleoid)
      @nucleoid = nucleoid
    end
  end
end
```

It gives you much better understanding of Domain in which we are operating. It also makes it much easier to understand for someone who knows something about DNA.
There could be also one more thing extracted to separate module and that's Hamming distance. Because as it turns out Hamming distance has nothing to do with DNA. In fact, it’s part of Information Theory and calculates differences between two strings.
During that task, I learned a lot about how we can represent parts of real worlds using OOP and how to not treat everything as strings, arrays, numbers, etc. because our world is much more complicated than that.
If you want to know more about it check out Arkency series in which Andrzej uses exactly the same exercise to explain basics of DDD.
[Part I](https://www.youtube.com/watch?v=h5UF4LkGBSk)
[Part II](https://www.youtube.com/watch?v=4j3IEUdgrv8)
