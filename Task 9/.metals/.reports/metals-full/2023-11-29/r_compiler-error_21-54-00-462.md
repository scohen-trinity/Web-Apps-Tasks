file:///C:/Users/sammy/Downloads/Web%20Apps/Task%209/play-tasks-scohen-trinity/build.sbt
### scala.MatchError: [I@13e6e9e6 (of class [I)

occurred in the presentation compiler.

action parameters:
offset: 1111
uri: file:///C:/Users/sammy/Downloads/Web%20Apps/Task%209/play-tasks-scohen-trinity/build.sbt
text:
```scala
import sbtcrossproject.{crossProject, CrossType}

enablePlugins(JavaAppPackaging)

Global / onChangedBuildSource := ReloadOnSourceChanges

lazy val server = (project in file("server")).settings(commonSettings).settings(
	name := "play-server",
  version := "0.1.0",
  scalaJSProjects := Seq(client),
  Assets / pipelineStages := Seq(scalaJSPipeline),
  pipelineStages := Seq(digest, gzip),
  // triggers scalaJSPipeline when using compile or continuous compilation
  Compile / compile := ((Compile / compile) dependsOn scalaJSPipeline).value,
  libraryDependencies ++= Seq(
    "com.vmunier" %% "scalajs-scripts" % "1.2.0",
    "com.google.inject"            % "guice"                % "6.0.0",
    "com.google.inject.extensions" % "guice-assistedinject" % "6.0.0",
    guice,
		"org.scalatestplus.play" %% "scalatestplus-play" % "5.1.0" % Test,
		"com.typesafe.play" %% "play-slick" % "5.1.0",
		"com.typesafe.slick" %% "slick-codegen" % "3.4.1",
    "org.postgresql" % "postgresql" % "42.6.0",
    "com.typesafe.slick" %% "slick-hikaricp" % "3.4.1",
    "org.mindrot" % "jbcrypt" % "0.@@"
    specs2 % Test
  ),
  Test / javaOptions ++= Seq(
    "--add-exports=java.base/sun.security.x509=ALL-UNNAMED",
    "--add-opens=java.base/sun.security.ssl=ALL-UNNAMED" // Could be needed as well in some cases
  ),
).enablePlugins(PlayScala).
  dependsOn(sharedJvm)

lazy val client = (project in file("client")).settings(commonSettings).settings(
	name := "play-client",
  scalacOptions += "-Ymacro-annotations",
  scalaJSUseMainModuleInitializer := true,
  libraryDependencies ++= Seq(
    "org.scala-js" %%% "scalajs-dom" % "2.6.0",
		"me.shadaj" %%% "slinky-core" % "0.7.4",
		"me.shadaj" %%% "slinky-web" % "0.7.4"
  )
).enablePlugins(ScalaJSPlugin, ScalaJSWeb).
  dependsOn(sharedJs)

lazy val shared = crossProject(JSPlatform, JVMPlatform)
  .crossType(CrossType.Pure)
  .in(file("shared"))
  .settings(commonSettings)
	.settings(
		name := "play-shared"
	)
lazy val sharedJvm = shared.jvm
lazy val sharedJs = shared.js

lazy val commonSettings = Seq(
  scalaVersion := "2.13.11",
  organization := "edu.trinity",
  libraryDependencies += "com.typesafe.play" %%% "play-json" % "2.9.4"
)

// loads the server project at sbt startup
onLoad in Global := (onLoad in Global).value andThen {s: State => "project server" :: s}

```



#### Error stacktrace:

```
scala.meta.internal.semver.SemVer$Version$.fromString(SemVer.scala:52)
	scala.meta.internal.mtags.CoursierComplete.$anonfun$versionCompletions$1(CoursierComplete.scala:68)
	scala.meta.internal.mtags.CoursierComplete.$anonfun$versionCompletions$1$adapted(CoursierComplete.scala:68)
	scala.math.Ordering$$anon$4.compare(Ordering.scala:234)
	java.base/java.util.TimSort.countRunAndMakeAscending(TimSort.java:355)
	java.base/java.util.TimSort.sort(TimSort.java:220)
	java.base/java.util.Arrays.sort(Arrays.java:1441)
	scala.collection.SeqLike.sorted(SeqLike.scala:659)
	scala.collection.SeqLike.sorted$(SeqLike.scala:647)
	scala.collection.AbstractSeq.sorted(Seq.scala:45)
	scala.collection.SeqLike.sortWith(SeqLike.scala:612)
	scala.collection.SeqLike.sortWith$(SeqLike.scala:612)
	scala.collection.AbstractSeq.sortWith(Seq.scala:45)
	scala.meta.internal.mtags.CoursierComplete.versionCompletions(CoursierComplete.scala:68)
	scala.meta.internal.mtags.CoursierComplete.complete(CoursierComplete.scala:47)
	scala.meta.internal.pc.completions.SbtLibCompletions$SbtLibCompletion.contribute(SbtLibCompletions.scala:71)
	scala.meta.internal.pc.CompletionProvider.filterInteresting(CompletionProvider.scala:353)
	scala.meta.internal.pc.CompletionProvider.safeCompletionsAt(CompletionProvider.scala:487)
	scala.meta.internal.pc.CompletionProvider.completions(CompletionProvider.scala:58)
	scala.meta.internal.pc.ScalaPresentationCompiler.$anonfun$complete$1(ScalaPresentationCompiler.scala:169)
```
#### Short summary: 

scala.MatchError: [I@13e6e9e6 (of class [I)