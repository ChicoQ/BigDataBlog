## Deploy the Latest Spark 2.1 on CDH

You can use the [editor on GitHub](https://github.com/ChicoQ/BigDataBlog/edit/master/DeploySpark2.1.md) to maintain and preview the content for your website in Markdown files.

I had followed this article [Running Spark 2.x.x on Cloudera Hadoop Distro (CDH)](https://www.linkedin.com/pulse/running-spark-2xx-cloudera-hadoop-distro-cdh-deenar-toraskar-cfa) and made some changes.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

#### Check the version of CDH and Hadoop running on your cluster using

```makedown
# hadoop version
Hadoop 2.6.0-cdh5.7.0
```

#### Download Spark's source code from [Spark] (http://spark.apache.org/downloads.html)

#### Compile

```markdown
-Phive -Phive-thriftserver -Pyarn
```

#### Change SPARK_HOME

#### Change configuration

```markdown

```

#### Copying Spark floder to all nodes

```
# scp
```

#### Finally test your new Spark

```markdown
./bin/run-example SparkP 10 --master yarn
./bin/spark-shell --master yarn
./bin/pyspark

```



```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/ChicoQ/BigDataBlog/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
