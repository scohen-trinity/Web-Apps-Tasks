file:///C:/Users/sammy/Downloads/Web%20Apps/Task%2011/play-tasks-scohen-trinity/client/src/main/scala/playscala/Task11.scala
### file%3A%2F%2F%2FC%3A%2FUsers%2Fsammy%2FDownloads%2FWeb%2520Apps%2FTask%252011%2Fplay-tasks-scohen-trinity%2Fclient%2Fsrc%2Fmain%2Fscala%2Fplayscala%2FTask11.scala:40: error: ; expected but , found
        val socketRoute = dom,
                             ^

occurred in the presentation compiler.

action parameters:
uri: file:///C:/Users/sammy/Downloads/Web%20Apps/Task%2011/play-tasks-scohen-trinity/client/src/main/scala/playscala/Task11.scala
text:
```scala
package playscala

import shared.SharedMessages

import org.scalajs.dom.document
import scalajs.js
import org.scalajs.dom
import org.scalajs.dom.html
import org.scalajs.dom.raw.MouseEvent
import scala.scalajs.js.annotation.JSExportTopLevel
import scala.scalajs.js.JSON
import play.api.libs.json._
import scala.scalajs.js.Dynamic.{literal => json}
import org.scalajs.dom.ext.Ajax
import scala.scalajs.concurrent.JSExecutionContext.Implicits.queue
import scala.annotation.varargs

object Task11 { 
    def main(args: Array[String]): Unit = {
        val width = 800;
        val height = 800;
        val canvas = dom.document.createElement("canvas").asInstanceOf[html.Canvas]
        canvas.height = height
        canvas.width = width
        canvas.style.border = "1px solid black"
        canvas.id = "painting"

        val target = document.getElementById("easel")
        dom.document.body.appendChild(canvas)

        Paintbrush.startPainting()
    }
}

object Paintbrush {
    @JSExportTopLevel("startPainting")
    def startPainting(): Unit = {
        var isPainting = false
        val canvas = dom.document.getElementById("painting").asInstanceOf[html.Canvas]
        val socketRoute = dom,
        val ctx = canvas.getContext("2d").asInstanceOf[dom.CanvasRenderingContext2D]

        val wsUrl = "ws://localhost:9000/canvasSocket"

        var drawingData = List.empty[(Int, Int)]
        var lineColor = "#FF0000"

        val webSocket = new dom.WebSocket(wsUrl)

        webSocket.onopen = { _: dom.Event =>
            println("Websocket connected")
            sendDataToServer(lineColor, drawingData)    
        }

        webSocket.onerror = { errorEvent: dom.Event =>
            val castedErrorEvent = errorEvent.asInstanceOf[dom.ErrorEvent]
            println(s"Websocket error: ${castedErrorEvent.message}")    
        }

        webSocket.onmessage = { messageEvent: dom.MessageEvent =>
            val receivedData = messageEvent.data.toString
            println(s"Received message from server: $receivedData")
            redrawCanvas(receivedData)    
        }

        webSocket.onclose = { closeEvent: dom.CloseEvent =>
            println("Websocket connection closed")    
        }

        def sendDataToServer(color: String, data: List[(Int, Int)]): Unit = {
            var lineData = (color, data)
            
            val serializedData = Json.toJson(lineData).toString()

            webSocket.send(serializedData)
        }

        def redrawCanvas(points: String): Unit = {
            
            val lines: List[(String, List[(Int, Int)])] = Json.parse(points).as[List[(String, List[(Int, Int)])]]
            println(lines)
            ctx.clearRect(0, 0, canvas.width, canvas.height)

            lines.foreach { line =>
                ctx.beginPath()
                ctx.strokeStyle = line._1
                if (line._2.nonEmpty) {
                    ctx.moveTo(line._2.head._1, line._2.head._2)
                }

                line._2.foreach { point =>
                    ctx.lineTo(point._1, point._2) 
                }
                ctx.stroke()
            }
        }

        def getRandomColor(): Unit = {
            val letters = "0123456789ABCDEF"
            val color = new StringBuilder("#")
            for (_ <- 0 until 6) {
                color.append(letters.charAt(Math.floor(Math.random() * 16).toInt))
            }
    
            lineColor = color.toString()
            println(lineColor)
        }

        canvas.onmousedown = (e: MouseEvent) => {
            val point = ((e.clientX - canvas.offsetLeft).toInt, (e.clientY - canvas.offsetTop).toInt)
            drawingData :+= point
            isPainting = true
            ctx.beginPath()
            ctx.moveTo(e.clientX - canvas.offsetLeft, e.clientY - canvas.offsetTop)
        }

        canvas.onmousemove = (e: MouseEvent) => {
            if(isPainting) {
                ctx.lineTo(e.clientX - canvas.offsetLeft, e.clientY - canvas.offsetTop)
                ctx.stroke()

                val point = ((e.clientX - canvas.offsetLeft).toInt, (e.clientY - canvas.offsetTop).toInt)
                drawingData :+= point
            }
        }

        canvas.onmouseup = (e: MouseEvent) => {
            if (isPainting) {
                isPainting = false
                sendDataToServer(lineColor, drawingData)
                drawingData = List.empty[(Int, Int)]
            }
        }

        dom.window.addEventListener("keydown", { (event: dom.KeyboardEvent) =>
            if (event.keyCode == dom.ext.KeyCode.Space) { 
                val randomColor = getRandomColor()
                ctx.strokeStyle = randomColor
            }
        })
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
	scala.meta.internal.parsers.ScalametaParser.acceptStatSep(ScalametaParser.scala:447)
	scala.meta.internal.parsers.ScalametaParser.acceptStatSepOpt(ScalametaParser.scala:451)
	scala.meta.internal.parsers.ScalametaParser.iter$6(ScalametaParser.scala:4599)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$blockStatSeq$1(ScalametaParser.scala:4620)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$blockStatSeq$1$adapted(ScalametaParser.scala:4570)
	scala.meta.internal.parsers.ScalametaParser.scala$meta$internal$parsers$ScalametaParser$$listBy(ScalametaParser.scala:568)
	scala.meta.internal.parsers.ScalametaParser.blockStatSeq(ScalametaParser.scala:4570)
	scala.meta.internal.parsers.ScalametaParser.blockWithinDelims(ScalametaParser.scala:2507)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$blockInDelims$2(ScalametaParser.scala:2510)
	scala.meta.internal.parsers.ScalametaParser.inBracesOnOpen(ScalametaParser.scala:264)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$blockOnBrace$1(ScalametaParser.scala:2516)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$blockInDelims$1(ScalametaParser.scala:2510)
	scala.meta.internal.parsers.ScalametaParser.atPos(ScalametaParser.scala:319)
	scala.meta.internal.parsers.ScalametaParser.autoPos(ScalametaParser.scala:365)
	scala.meta.internal.parsers.ScalametaParser.blockInDelims(ScalametaParser.scala:2510)
	scala.meta.internal.parsers.ScalametaParser.blockOnBrace(ScalametaParser.scala:2516)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$blockExprOnBrace$2(ScalametaParser.scala:2519)
	scala.meta.internal.parsers.ScalametaParser.blockExprPartial(ScalametaParser.scala:2503)
	scala.meta.internal.parsers.ScalametaParser.blockExprOnBrace(ScalametaParser.scala:2519)
	scala.meta.internal.parsers.ScalametaParser.simpleExpr0(ScalametaParser.scala:2272)
	scala.meta.internal.parsers.ScalametaParser.simpleExpr(ScalametaParser.scala:2243)
	scala.meta.internal.parsers.ScalametaParser.prefixExpr(ScalametaParser.scala:2226)
	scala.meta.internal.parsers.ScalametaParser.postfixExpr(ScalametaParser.scala:2100)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$expr$2(ScalametaParser.scala:1682)
	scala.meta.internal.parsers.ScalametaParser.atPosOpt(ScalametaParser.scala:322)
	scala.meta.internal.parsers.ScalametaParser.autoPosOpt(ScalametaParser.scala:366)
	scala.meta.internal.parsers.ScalametaParser.expr(ScalametaParser.scala:1587)
	scala.meta.internal.parsers.ScalametaParser.expr(ScalametaParser.scala:1486)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$funDefRest$1(ScalametaParser.scala:3827)
	scala.meta.internal.parsers.ScalametaParser.autoEndPos(ScalametaParser.scala:368)
	scala.meta.internal.parsers.ScalametaParser.autoEndPos(ScalametaParser.scala:373)
	scala.meta.internal.parsers.ScalametaParser.funDefRest(ScalametaParser.scala:3789)
	scala.meta.internal.parsers.ScalametaParser.funDefOrDclOrExtensionOrSecondaryCtor(ScalametaParser.scala:3734)
	scala.meta.internal.parsers.ScalametaParser.defOrDclOrSecondaryCtor(ScalametaParser.scala:3564)
	scala.meta.internal.parsers.ScalametaParser.nonLocalDefOrDcl(ScalametaParser.scala:3543)
	scala.meta.internal.parsers.ScalametaParser$$anonfun$templateStat$1.applyOrElse(ScalametaParser.scala:4517)
	scala.meta.internal.parsers.ScalametaParser$$anonfun$templateStat$1.applyOrElse(ScalametaParser.scala:4511)
	scala.PartialFunction.$anonfun$runWith$1$adapted(PartialFunction.scala:145)
	scala.meta.internal.parsers.ScalametaParser.statSeqBuf(ScalametaParser.scala:4462)
	scala.meta.internal.parsers.ScalametaParser.getStats$2(ScalametaParser.scala:4501)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$scala$meta$internal$parsers$ScalametaParser$$templateStatSeq$3(ScalametaParser.scala:4502)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$scala$meta$internal$parsers$ScalametaParser$$templateStatSeq$3$adapted(ScalametaParser.scala:4499)
	scala.meta.internal.parsers.ScalametaParser.scala$meta$internal$parsers$ScalametaParser$$listBy(ScalametaParser.scala:568)
	scala.meta.internal.parsers.ScalametaParser.scala$meta$internal$parsers$ScalametaParser$$templateStatSeq(ScalametaParser.scala:4499)
	scala.meta.internal.parsers.ScalametaParser.scala$meta$internal$parsers$ScalametaParser$$templateStatSeq(ScalametaParser.scala:4491)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$templateBody$1(ScalametaParser.scala:4342)
	scala.meta.internal.parsers.ScalametaParser.inBracesOr(ScalametaParser.scala:260)
	scala.meta.internal.parsers.ScalametaParser.inBraces(ScalametaParser.scala:256)
	scala.meta.internal.parsers.ScalametaParser.templateBody(ScalametaParser.scala:4342)
	scala.meta.internal.parsers.ScalametaParser.templateBodyOpt(ScalametaParser.scala:4346)
	scala.meta.internal.parsers.ScalametaParser.templateAfterExtends(ScalametaParser.scala:4289)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$templateOpt$1(ScalametaParser.scala:4337)
	scala.meta.internal.parsers.ScalametaParser.atPos(ScalametaParser.scala:319)
	scala.meta.internal.parsers.ScalametaParser.autoPos(ScalametaParser.scala:365)
	scala.meta.internal.parsers.ScalametaParser.templateOpt(ScalametaParser.scala:4327)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$objectDef$1(ScalametaParser.scala:4027)
	scala.meta.internal.parsers.ScalametaParser.autoEndPos(ScalametaParser.scala:368)
	scala.meta.internal.parsers.ScalametaParser.autoEndPos(ScalametaParser.scala:373)
	scala.meta.internal.parsers.ScalametaParser.objectDef(ScalametaParser.scala:4019)
	scala.meta.internal.parsers.ScalametaParser.tmplDef(ScalametaParser.scala:3896)
	scala.meta.internal.parsers.ScalametaParser.topLevelTmplDef(ScalametaParser.scala:3877)
	scala.meta.internal.parsers.ScalametaParser$$anonfun$2.applyOrElse(ScalametaParser.scala:4483)
	scala.meta.internal.parsers.ScalametaParser$$anonfun$2.applyOrElse(ScalametaParser.scala:4471)
	scala.PartialFunction.$anonfun$runWith$1$adapted(PartialFunction.scala:145)
	scala.meta.internal.parsers.ScalametaParser.statSeqBuf(ScalametaParser.scala:4462)
	scala.meta.internal.parsers.ScalametaParser.bracelessPackageStats$1(ScalametaParser.scala:4681)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$batchSource$11(ScalametaParser.scala:4692)
	scala.meta.internal.parsers.ScalametaParser.autoEndPos(ScalametaParser.scala:368)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$batchSource$10(ScalametaParser.scala:4692)
	scala.meta.internal.parsers.ScalametaParser.tryParse(ScalametaParser.scala:216)
	scala.meta.internal.parsers.ScalametaParser.$anonfun$batchSource$1(ScalametaParser.scala:4684)
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

file%3A%2F%2F%2FC%3A%2FUsers%2Fsammy%2FDownloads%2FWeb%2520Apps%2FTask%252011%2Fplay-tasks-scohen-trinity%2Fclient%2Fsrc%2Fmain%2Fscala%2Fplayscala%2FTask11.scala:40: error: ; expected but , found
        val socketRoute = dom,
                             ^