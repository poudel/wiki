+++
title = "AoC 2020, Day 2"
weight = 101
+++

Day two of the advent calendar, I decided not to do it first in python
to focus only on Clojure.

I put the repo with all solutions on github. But I'm not beneath
adding some padding to blog.


## Part One {#part-one}

This is my first time doing the advent of code, even though it has
been going on for about 5 years.

Which is why I didn't realize that every challenge has more than one
parts. Just like yesterday.

This is what I came up with. It took me a lot more than I'd
anticipated.

```clojure
(def regex #"(?<min>\d+)-(?<max>\d+)\s+(?<letter>\w):\s+(?<password>.*)")
(def input-file-path (str (.getCanonicalPath (clojure.java.io/file ".")) "/resources/day2-input.txt"))


(defn parse-line [line]
  "Parse each line and return a vector containing
  [min max char password] or nil"

  (let [[_ min max letter password]
        (re-matches regex line)]

    (if (and min max letter password)
      [(Integer/parseInt min)
       (Integer/parseInt max)
       (first (char-array letter))
       password])))


(defn is-valid?
  "Predicate. Checks if a given password string
  adheres to the given password policy."

  [line]

  (let [[min max char password :as parsed] (parse-line line)
        lcount (get (frequencies password) char 0)]

    (boolean
     (and
      parsed
      (<= lcount max)
      (>= lcount min)))))


(defn part1 []
   (count
    (filter
     is-valid?
     (line-seq (clojure.java.io/reader input-file-path)))))
```


## Part Two {#part-two}

Most of the functions defined in part one were usable in part two as
well. I didn't have to refactor everything like I had to yesterday.

```clojure
(defn is-valid-two?
  [line]
  (let [[x y char password :as parsed] (parse-line line)
        password-arr (char-array password)
        x-exists (= (get password-arr (- x 1)) char)
        y-exists (= (get password-arr (- y 1)) char)]
    (and (or x-exists y-exists)
         (not (and x-exists y-exists)))))


(defn part2 []
  (count
   (filter
    is-valid-two?
    (line-seq (clojure.java.io/reader input-file-path)))))
```

There is a little bit of duplication between part1 and part2 but it's
something that I can live with.


## Notes {#notes}

I learned how to unpack (a.k.a. destructure in clojure land) a
vector (or any sequence) in arguments or in a `let`.

For challenge part2, I might be able to make it a little more elegant.
But that's for the future.

I also did some re-organization so that `lein run` command that runs
the `main` method from `core` can be used to run solutions to all
challenges.

Using regex was fairly simple. My favorite regex101.com doesn
explicitly have support for Clojure (or Java, for that matter) but
JavaScript regex engine is very similar to the one in Java.

The \`slurp\` function is amazing.


## Links {#links}

1.  [advent-2020-input-day-two.txt](../static/advent-2020-input-day-two.txt)
2.  [Day 1]({{< relref "advent-of-code-2020-day-one" >}}) of the challenge
3.  [github](https://github.com/poudel/advent-2020-clj) repo of the solution
