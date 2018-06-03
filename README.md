# It-s-Probably-Nothing-Repo 我自一口真气足

<h3>pass-by-value vs pass-by-ref</h3>
* pass-by-value: pass in the variable that points to the address. Any changes to the variable/content of address will affect caller.
* pass-by-ref: pass in a new variable that points to the address. Changes to the variable will not reflect back to the caller. Only content change will reflect, because the two addresses are pointing to the same object.

http://www.hollischuang.com/
this person is indeed celery


# Short cut to check what jar doesn't exist in corp artifactory repo

<h4>The corp repo has limited third party jars. The developer built the project locally connecting to Jcenter & others. But in build pipeline we shouldn't. To check what jar are missing:</h4>
<p>Remove the maven pom "dependencyManagement". Doing so will unroll/unmarshal dependencies.</p>
<a href="https://stackoverflow.com/a/37280943/6641032">Dependency Management VS dependencies</a>

```Gradle
publishing {
	publications {
		mavenJava(MavenPublication) {
			groupId group
			artifactId artifactName
			version = snapshotVersion
			artifact makeZip
			
			from components.java
			pom.withXml {
				Node pomNode = asNode()
				pomNode.remove(pomNode.get("dependencyManagement"))
			}
		}
	}
}
```

Now running the Gradle publish again will start to complain all missing jars instead of just the missing root level hierachy pom.


# HTTP长连接和短连接
<a href="http://www.cnblogs.com/0201zcr/p/4694945.html">Link</a>
<p>长连结saves connection time -- only disconnect upon time out. But heavier weight; 短链接 load大量时常用</p>

# Front-end back-end decoupling
https://blog.csdn.net/piantoutongyang/article/details/65446892

# Why use Webpack
<a href="https://blog.andrewray.me/webpack-when-to-use-and-why/">Long/full story</a>
<p>It's a build tool to manage assets: Js, imgs, CSS etc.</p>
<h4>Ver. 1</h4>

```javascript
<script src="jquery.min.js"></script>  
<script src="jquery.some.plugin.js"></script>  
<script src="main.js"></script>  
```

<h4>Ver. 2</h4>

```javascript
// build-script.js
var scripts = [  
    'jquery.min.js',
    'jquery.some.plugin.js',
    'main.js'
].concat().uglify().writeTo('bundle.js');

// Everything our app needs!
<script src="bundle.js"></script>  
```

<a href="https://webpack.js.org/guides/getting-started/"><h4>Then the webpack way</h4></a>


# Queue, Dequeue, and why not stack
<p>Queue interface:</p>

| Queue         | Throw Excpetion| Return flag val|
| ------------- |:--------------:| --------------:|
| Insert        | add(e)         | offer(e)       |
| Remove        | remove()       | poll()         |
| Check         | elment()       | peek()         |

Notable implementations: ArrayList, Dequeue

<p>Deque class:</p>
<em>Two ended queue.</em>

| Deque         | Throw Excpetion             | Return flag val            |
| ------------- |:---------------------------:| --------------------------:|
| Insert        | addFirst(e)/addLast(e)      | offerFirst(e)/offerLast(e) |
| Remove        | removeFirst()/removeLast()  | pollFirst()/pollLast()     |
| Check         | getFirst()/getLast()        | peekFirst()/peekLast()     |

<p>Stack</p>
Stack inherits Vector class, which many consider an overuse of inheritance. It's now legacy stack impl. Now should use Deque when need a stack.
There are second opinions like <a href="http://baddotrobot.com/blog/2013/01/10/stack-vs-deque/">this</a>. Since Deque is NOT a stack. 
