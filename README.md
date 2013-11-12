Animate me LESS
===============

### Keyframe & animation magic mixin for less

#### Animate me less or more helps you out generating a cross-browser and completely valid @keyframes & animation's css code, without hacks.

>Why?
I usually prefer SCSS but since less is better supported in wordpress ([less-php](http://leafo.net/lessphp/), [wp-less](https://github.com/oncletom/wp-less/wiki))
and I wanted to simplify [this huge css](http://tympanus.net/Development/ResponsiveMultiLevelMenu/css/component.css)
to recreate [this nice menu](http://tympanus.net/Development/ResponsiveMultiLevelMenu/) I though to do this, to me useful, mixin.


How to use it
-------------------------
**First two things**
* Import the `animate-mi.less` file in your project (the code is kinda of minified, there's no special reason for this, but even if I haven't tested it, it might result in slightly faster compile process due to to the length of the variables names, but probably the advantage is really small, anyway at least it looks more compat and it takes less space).
You can always use the `animate-mi.dev.less` to take a look at the source code in a more readable way, and maybe improve it or customize your version.
* Remember always to declare (once is enough of course) this variable somewhere in your less files, setting the value accordingly with the compiler you use (`'js'` or `'php'` are the only ones supported):

```
@lesscompiler: 'js';
```

**Then you can write an animation in these ways:**


* declaring the keyframes and the **css selector** which will contain the animation

```
.animate-me(CrossFade; false; '.cross-fade';
	'0%, 100%'; 'opacity: .4'
);
```
* declaring also the **animation properties** you want *(like in plain css)*

```
.animate-me(CrossFade; 3s infinite linear; '.cross-fade';
	'0%, 100%'; 'opacity: .4'
);
```
* just declaring the **keyframes** *(withous css selector)*

```
.animate-me(TrafficLight; false; false;
	0%; 'background: red';
	20%; 'background: yellow';
	50%; 'background: green'
);
```
* if you want to animate properties that need to be prefixed you can put `%s` in front of the propertyName, without spaces in between, it will be replaced with the right **vendor prefix** only inside the relative vendor keyframe declaration

```
.animate-me(SimpleAnimation; false; '.simple-animation';
	0%; '%stransform: translateZ(0%) rotateY(0deg)';
	100%; '%stransform: translateZ(100%) rotateY(360deg)'
);
```
* you can put a lot of keyframes steps (percentages), but there's a limit: **10 steps/percentages maximum** *(if you write `'10%, 45%'` it count as one)*.

```
.animate-me(ComplexAnimation; 0.4s ease; '.complex-animation';
	'50%, 100%'; '%stransform: translateZ(-250px) rotateY(30deg)';
	70%; '%stransform: translateZ(-250px) rotateY(30deg); opacity: .5';
	30%; '%stransform: translateZ(-250px) rotateY(30deg); opacity: .5';
	80%; '%stransform: translateZ(-250px) rotateY(30deg); opacity: 1'
);
```

Result and considerations
-------------------------
As said before this mixin doesn't produce hacks, at least if the less compiled with the original javascript compiler,
* The only kind of 'hack' that you'll find in the generated css are <code>/* comments */</code>. This is normally not a problem, since they are ignored by the browsers.
* Anyway you probably always [minify your css](http://cssminifier.com/) in production so the generated comments will be stripped out.
* The generated code is completely valid and linted css: try it with [this useful tool](http://csslint.net/).

There are some differences if you compile your less code through the [less-php port](http://leafo.net/lessphp/) by @[leafo](https://github.com/leafo)
* the generated css have longer comments
* less readable formatting (linebrakes, spaces..) but since you minify it, who cares
* you'll find the code `.hack { visibility: visible }` at the end of all the rules of every animation only if the mixin is used without a css selector (so with false) on the third parameter


this piece of .less code...
```
@lesscompiler: 'js';


.animate-me(CrossFade; false; '.cross-fade';
	'0%, 100%'; 'opacity: .4'
);
```
...will output this css code with the **JS COMPILER**:

```
@-moz-keyframes CrossFade {
 0%, 100% {
    opacity: .4; /* {
  /**/
}
}
@-webkit-keyframes CrossFade {
 0%, 100% {
    opacity: .4; /* {
  /**/
}
}
@-o-keyframes CrossFade {
 0%, 100% {
    opacity: .4; /* {
  /**/
}
}
@keyframes CrossFade {
 0%, 100% {
    opacity: .4; /* {
  /**/
}
} .cross-fade {
  -moz-animation: CrossFade;
  -webkit-animation: CrossFade;
  -o-animation: CrossFade;
  animation: CrossFade;
}
```

...and this css code with the **PHP COMPILER**:
```
@lesscompiler: 'php';


@-moz-keyframes CrossFade { 0%, 100% { opacity: .4; /* {
  commented: 0;
}
*/} }@-webkit-keyframes CrossFade { 0%, 100% { opacity: .4; /* {
  commented: 0;
}
*/} }@-o-keyframes CrossFade { 0%, 100% { opacity: .4; /* {
  commented: 0;
}
*/} }@keyframes CrossFade { 0%, 100% { opacity: .4; /* {
  commented: 0;
}
*/} } .cross-fade {
  -moz-animation: CrossFade;
  -webkit-animation: CrossFade;
  -o-animation: CrossFade;
  animation: CrossFade;
}
```

Try it yourself here:
* the **[jsCompiler](http://less2css.org/)**
* the **[phpCompiler](http://leafo.net/lessphp/)**

Usage and parameters
-------------------------
These are the parameters you pass to the mixin:

<table>
  <tr>
    <th>name</th><th>properties</th><th>css selector</th><th>percentage</th><th>properties</th>
  </tr>
  <tr>
    <td><code>theAnimationName</code> <i>required</i></td>
    <td><code>false</code> <i>can be false</i></td>
    <td><code>false</code> <i>can be false</i></td>
    <td><code>number%</code> <i>without quotes</i></td>
    <td><code>%s</code> <i>in front of the css property to add the vendor prefix</i></td>
  </tr>
  <tr>
    <td></td>
    <td><code>0.5s</code> <i>see <a href="http://www.css3files.com/animation/">here</a> for all the animation properties</i></td>
    <td><code>'.css-animation-selector'</code> <i>must be quoted</i></td>
    <td><code>'15%, 75%'</code> <i>put quotes when you have more than one percentage</i></td>
    <td><i>can be one or as many properties...</i></td>
  </tr>
  <tr>
    <td></td>
    <td><code>4s ease 1s infinite</code></td>
    <td></td>
    <td><code>'from, to'</code> <i>put quotes when you want to use the 'from/to' notation</i></td>
    <td><i>...divide them with a semicolomn, like css.</i></td>
  </tr>
</table>


Thanks to these threads on stackoverflow:
* http://stackoverflow.com/questions/9166152/sign-and-variables-in-css-keyframes-using-less-css
* http://stackoverflow.com/questions/14868042/using-variables-in-property-names-in-less-dynamic-properties-property-name-in/15443008#15443008

