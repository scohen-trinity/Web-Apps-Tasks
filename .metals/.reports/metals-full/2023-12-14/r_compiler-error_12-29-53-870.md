file:///C:/Users/sammy/Downloads/Web%20Apps/Task%2011/play-tasks-scohen-trinity/client/src/main/scala/playscala/Task11.scala
### java.lang.NullPointerException

occurred in the presentation compiler.

action parameters:
offset: 3234
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

        def getRandomColor()@@

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
scala.reflect.internal.Definitions$DefinitionsClass.isByNameParamType(Definitions.scala:417)
	scala.reflect.internal.TreeInfo.isStableIdent(TreeInfo.scala:140)
	scala.reflect.internal.TreeInfo.isStableIdentifier(TreeInfo.scala:113)
	scala.reflect.internal.TreeInfo.isPath(TreeInfo.scala:102)
	scala.reflect.internal.TreeInfo.admitsTypeSelection(TreeInfo.scala:158)
	scala.tools.nsc.interactive.Global.stabilizedType(Global.scala:960)
	scala.tools.nsc.interactive.Global.typedTreeAt(Global.scala:808)
	scala.meta.internal.pc.SignatureHelpProvider.signatureHelp(SignatureHelpProvider.scala:23)
	scala.meta.internal.pc.ScalaPresentationCompiler.$anonfun$signatureHelp$1(ScalaPresentationCompiler.scala:282)
```
#### Short summary: 

java.lang.NullPointerException