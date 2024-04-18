file:///C:/Users/sammy/Downloads/Web%20Apps/Task%205/play-tasks-scohen-trinity/server/test/MessageList1Spec.scala
### file%3A%2F%2F%2FC%3A%2FUsers%2Fsammy%2FDownloads%2FWeb%2520Apps%2FTask%25205%2Fplay-tasks-scohen-trinity%2Fserver%2Ftest%2FMessageList1Spec.scala:6: error: . expected but \n\n found

 ^

occurred in the presentation compiler.

action parameters:
uri: file:///C:/Users/sammy/Downloads/Web%20Apps/Task%205/play-tasks-scohen-trinity/server/test/MessageList1Spec.scala
text:
```scala
import org.scalatestplus.play.PlaySpec
import org.scalatestplus.play.guice.GuiceOneServerPerSuite
import org.scalatestplus.play.OneBrowserPerSuite
import org.scalatestplus.play.HtmlUnitFactory
import seleniu

class MessageList1Spec extends PlaySpec with GuiceOneServerPerSuite with OneBrowserPerSuite with HtmlUnitFactory {
    "Message List 1" must {
        "login and access functions" in {
            go to s"http://localhost:$port/login1"

            click on "username-login"
            textField("username").value = "mlewis"
            click on "password-login"
            textField("password").value = "prof"
            submit()
        }
    }
}
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
	scala.meta.internal.parsers.ScalametaParser.$anonfun$importer$1(ScalametaParser.scala:3460)
	scala.meta.internal.parsers.ScalametaParser.atPos(ScalametaParser.scala:319)
	scala.meta.internal.parsers.ScalametaParser.autoPos(ScalametaParser.scala:365)
	scala.meta.internal.parsers.ScalametaParser.importer(ScalametaParser.scala:3438)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$importStmt$2(ScalametaParser.scala:3435)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$commaSeparated$1(ScalametaParser.scala:656)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$commaSeparated$1$adapted(ScalametaParser.scala:656)
	scala.meta.internal.parsers.ScalametaParser.iter$1(ScalametaParser.scala:646)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$tokenSeparated$1(ScalametaParser.scala:652)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$tokenSeparated$1$adapted(ScalametaParser.scala:639)
	scala.meta.internal.parsers.ScalametaParser.scala$meta$internal$parsers$ScalametaParser$$listBy(ScalametaParser.scala:568)
	scala.meta.internal.parsers.ScalametaParser.tokenSeparated(ScalametaParser.scala:639)
	scala.meta.internal.parsers.ScalametaParser.commaSeparatedWithIndex(ScalametaParser.scala:659)
	scala.meta.internal.parsers.ScalametaParser.commaSeparated(ScalametaParser.scala:656)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$importStmt$1(ScalametaParser.scala:3435)
	scala.meta.internal.parsers.ScalametaParser.atPos(ScalametaParser.scala:319)
	scala.meta.internal.parsers.ScalametaParser.autoPos(ScalametaParser.scala:365)
	scala.meta.internal.parsers.ScalametaParser.importStmt(ScalametaParser.scala:3433)
	scala.meta.internal.parsers.ScalametaParser$$anonfun$2.applyOrElse(ScalametaParser.scala:4479)
	scala.meta.internal.parsers.ScalametaParser$$anonfun$2.applyOrElse(ScalametaParser.scala:4471)
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

file%3A%2F%2F%2FC%3A%2FUsers%2Fsammy%2FDownloads%2FWeb%2520Apps%2FTask%25205%2Fplay-tasks-scohen-trinity%2Fserver%2Ftest%2FMessageList1Spec.scala:6: error: . expected but \n\n found

 ^