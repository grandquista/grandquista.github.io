---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: default
---

[Personal](/about)

# Projects

[chimera](https://github.com/grandquista/chimera) Python3 interpreter in C++.

[Lua-ReQL](https://github.com/grandquista/Lua-ReQL) RethinkDB driver for luarocks environment.

[CaaSbootstrap](https://github.com/grandquista/CaaSbootstrap) microservice intended to bootstrap itself into a compiler.

[primes](https://github.com/grandquista/primes) Sieve of Eratosthenes implemented in golang.

[glowing-chainsaw](https://github.com/grandquista/glowing-chainsaw) markov chain compression algorithm to practice reversible transformations.

# Posts

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

