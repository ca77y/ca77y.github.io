---
layout: post
title: Local maven repo in Ivy
tags: [maven, ivy]
---

For my limited experience I simply don't like Ant+Ivy. Sure, it's flexible and all but it's just too much xml for my
taste. Regardless I've been given two projects, one with ivy, one with maven.

Simple task:

* build maven project
* build ivy projects using the code from maven build

Hard solution :( Thing is I couldn't find any good example of Ivy using my local .m2 folder as a repository. If you
ask Uncle Google will give you a few interesting blog posts. The short story is a few posts on stackoverflow and some
guy raging about Ivy configuration which would be as follows:

{% highlight xml %}
<ivysettings>
    <settings defaultResolver="default"/>
    <property name="m2-pattern" value="${user.home}/.m2/repository/[organisation]/[module]/[revision]/[module]-[revision](-[classifier]).[ext]" override="false" />
    <resolvers>
        <chain name="default">
            <filesystem name="local-maven2" m2compatible="true" >
                <artifact pattern="${m2-pattern}"/>
                <ivy pattern="${m2-pattern}"/>
            </filesystem>
            <ibiblio name="central" m2compatible="true"/>
        </chain>
    </resolvers>
</ivysettings>
{% endhighlight %}

First of all this won't work, today that is. It will not parse pom.xml correctly and you will end up having 'almost'
empty ivy.xml. You can fix it by changing the pattern used in artifact tag to the same one but ending with .pom and
leaving ivy pattern as is.

Obviously this doesn't help and the story is long already. The solution is simple like that:

{% highlight xml %}
<!-- for getting stuff -->
<ibiblio name="local-m2" m2compatible="true" root="file://${user.home}/.m2/repository/" changingPattern=".*SNAPSHOT">
<!-- for putting stuff -->
<filesystem name="local-m2-publish" m2compatible="true">
      <artifact pattern="${user.home}/.m2/repository/[organisation]/[module]/[revision]/[artifact]-[revision](-[classifier]).[ext]"/>
</filesystem>
{% endhighlight %}

Add this to the resolvers/chain and live a happy life.