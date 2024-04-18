file:///C:/Users/sammy/Downloads/Web%20Apps/Task%205/play-tasks-scohen-trinity/build.sbt
### file%3A%2F%2F%2FC%3A%2FUsers%2Fsammy%2FDownloads%2FWeb%2520Apps%2FTask%25205%2Fplay-tasks-scohen-trinity%2Fbuild.sbt:60: error: ) expected but string constant found
  libraryDependencies += ""
                         ^

occurred in the presentation compiler.

action parameters:
uri: file:///C:/Users/sammy/Downloads/Web%20Apps/Task%205/play-tasks-scohen-trinity/build.sbt
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
  libraryDependencies += ""

)

// loads the server project at sbt startup
onLoad in Global := (onLoad in Global).value andThen {s: State => "project server" :: s}

```



#### Error stacktrace:

```
scala.meta.internal.parsers.Reporter.syntaxError(Reporter.scala:16)
	scala.meta.internal.parsers.Reporter.syntaxError$(Reporter.scala:16)
	scala.meta.internal.parsers.Reporter$$anon$1.syntaxError(Reporter.scala:22)
	scala.meta.internal.parsers.Reporter.syntaxError(Reporter.scala:17)
	scala.meta.internal.parsers.Reporter.syntaxError$(Reporter.scala:17)
	scala.meta.internal.parsers.Reporter$$anon$1.syntaxError(Reporter.scala:22)
	scala.meta.internal.parsers.ScalametaParser.syntaxErrorExpected(ScalametaParser.scala:421)
	scala.meta.internal.parsers.ScalametaParser.expect(ScalametaParser.scala:423)
	scala.meta.internal.parsers.ScalametaParser.accept(ScalametaParser.scala:427)
	scala.meta.internal.parsers.ScalametaParser.scala$meta$internal$parsers$ScalametaParser$$inParensAfterOpen(ScalametaParser.scala:248)
	scala.meta.internal.parsers.ScalametaParser.scala$meta$internal$parsers$ScalametaParser$$inParensAfterOpenOr(ScalametaParser.scala:253)
	scala.meta.internal.parsers.ScalametaParser.scala$meta$internal$parsers$ScalametaParser$$inParensOnOpenOr(ScalametaParser.scala:244)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$scala$meta$internal$parsers$ScalametaParser$$getArgClause$1(ScalametaParser.scala:2468)
	scala.meta.internal.parsers.ScalametaParser.atPos(ScalametaParser.scala:319)
	scala.meta.internal.parsers.ScalametaParser.autoPos(ScalametaParser.scala:365)
	scala.meta.internal.parsers.ScalametaParser.scala$meta$internal$parsers$ScalametaParser$$getArgClause(ScalametaParser.scala:2457)
	scala.meta.internal.parsers.ScalametaParser.simpleExprRest(ScalametaParser.scala:2364)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$simpleExpr0$3(ScalametaParser.scala:2293)
	scala.util.Success.$anonfun$map$1(Try.scala:255)
	scala.util.Success.map(Try.scala:213)
	scala.meta.internal.parsers.ScalametaParser.simpleExpr0(ScalametaParser.scala:2293)
	scala.meta.internal.parsers.ScalametaParser.simpleExpr(ScalametaParser.scala:2243)
	scala.meta.internal.parsers.ScalametaParser.prefixExpr(ScalametaParser.scala:2226)
	scala.meta.internal.parsers.ScalametaParser.postfixExpr(ScalametaParser.scala:2100)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$expr$2(ScalametaParser.scala:1682)
	scala.meta.internal.parsers.ScalametaParser.atPosOpt(ScalametaParser.scala:322)
	scala.meta.internal.parsers.ScalametaParser.autoPosOpt(ScalametaParser.scala:366)
	scala.meta.internal.parsers.ScalametaParser.expr(ScalametaParser.scala:1587)
	scala.meta.internal.parsers.ScalametaParser.expr(ScalametaParser.scala:1486)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$patDefOrDcl$1(ScalametaParser.scala:3609)
	scala.meta.internal.parsers.ScalametaParser.autoEndPos(ScalametaParser.scala:368)
	scala.meta.internal.parsers.ScalametaParser.autoEndPos(ScalametaParser.scala:373)
	scala.meta.internal.parsers.ScalametaParser.patDefOrDcl(ScalametaParser.scala:3596)
	scala.meta.internal.parsers.ScalametaParser.defOrDclOrSecondaryCtor(ScalametaParser.scala:3558)
	scala.meta.internal.parsers.ScalametaParser.nonLocalDefOrDcl(ScalametaParser.scala:3543)
	scala.meta.internal.parsers.ScalametaParser$$anonfun$1.applyOrElse(ScalametaParser.scala:4404)
	scala.meta.internal.parsers.ScalametaParser$$anonfun$1.applyOrElse(ScalametaParser.scala:4399)
	scala.PartialFunction.$anonfun$runWith$1$adapted(PartialFunction.scala:145)
	scala.meta.internal.parsers.ScalametaParser.statSeqBuf(ScalametaParser.scala:4462)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$batchSource$13(ScalametaParser.scala:4696)
	scala.Option.getOrElse(Option.scala:189)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$batchSource$1(ScalametaParser.scala:4696)
	scala.meta.internal.parsers.ScalametaParser.atPos(ScalametaParser.scala:319)
	scala.meta.internal.parsers.ScalametaParser.autoPos(ScalametaParser.scala:365)
	scala.meta.internal.parsers.ScalametaParser.batchSource(ScalametaParser.scala:4652)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$source$1(ScalametaParser.scala:4645)
	scala.meta.internal.parsers.ScalametaParser.atPos(ScalametaParser.scala:319)
	scala.meta.internal.parsers.ScalametaParser.autoPos(ScalametaParser.scala:365)
	scala.meta.internal.parsers.ScalametaParser.source(ScalametaParser.scala:4645)
	scala.meta.internal.parsers.ScalametaParser.entrypointSource(ScalametaParser.scala:4650)
	scala.meta.internal.parsers.ScalametaParser.parseSourceImpl(ScalametaParser.scala:135)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$parseSource$1(ScalametaParser.scala:132)
	scala.meta.internal.parsers.ScalametaParser.parseRuleAfterBOF(ScalametaParser.scala:59)
	scala.meta.internal.parsers.ScalametaParser.parseRule(ScalametaParser.scala:54)
	scala.meta.internal.parsers.ScalametaParser.parseSource(ScalametaParser.scala:132)
	scala.meta.parsers.Parse$.$anonfun$parseSource$1(Parse.scala:29)
	scala.meta.parsers.Parse$$anon$1.apply(Parse.scala:36)
	scala.meta.parsers.Api$XtensionParseDialectInput.parse(Api.scala:25)
	scala.meta.internal.semanticdb.scalac.ParseOps$XtensionCompilationUnitSource.toSource(ParseOps.scala:17)
	scala.meta.internal.semanticdb.scalac.TextDocumentOps$XtensionCompilationUnitDocument.toTextDocument(TextDocumentOps.scala:206)
	scala.meta.internal.pc.SemanticdbTextDocumentProvider.textDocument(SemanticdbTextDocumentProvider.scala:54)
	scala.meta.internal.pc.ScalaPresentationCompiler.$anonfun$semanticdbTextDocument$1(ScalaPresentationCompiler.scala:356)
```
#### Short summary: 

file%3A%2F%2F%2FC%3A%2FUsers%2Fsammy%2FDownloads%2FWeb%2520Apps%2FTask%25205%2Fplay-tasks-scohen-trinity%2Fbuild.sbt:60: error: ) expected but string constant found
  libraryDependencies += ""
                         ^