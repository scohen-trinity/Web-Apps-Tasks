file:///C:/Users/sammy/Downloads/Web%20Apps/Task%209/play-tasks-scohen-trinity/server/app/controllers/messageList.scala
### java.lang.IndexOutOfBoundsException: 0

occurred in the presentation compiler.

action parameters:
offset: 4218
uri: file:///C:/Users/sammy/Downloads/Web%20Apps/Task%209/play-tasks-scohen-trinity/server/app/controllers/messageList.scala
text:
```scala
package controllers

import javax.inject._

import play.api.mvc._
import play.api.i18n._
import models.MessageListInMemoryModel
import play.api.libs.json._
import models._

import play.api.db.slick.DatabaseConfigProvider
import scala.concurrent.ExecutionContext
import play.api.db.slick.HasDatabaseConfigProvider
import slick.jdbc.JdbcProfile
import slick.jdbc.PostgresProfile.api._
import scala.concurrent.Future
import play.api.Logger

@Singleton
class messageList @Inject() (protected val dbConfigProvider: DatabaseConfigProvider,  cc: ControllerComponents)(implicit ec: ExecutionContext) 
        extends AbstractController(cc) with HasDatabaseConfigProvider[JdbcProfile] {

    private val model = new MessageListDatabaseModel(db)
    private val logger = Logger(getClass)

    def load = Action {implicit request =>
        Ok(views.html.reactMessagesMain())
    }

    implicit val userDataReads = Json.reads[UserData]

    def withJsonBody[A](f: A => Future[Result]) (implicit request: Request[AnyContent], reads: Reads[A]): Future[Result] = {
        request.body.asJson.map { body =>
            Json.fromJson[A](body) match {
                case JsSuccess(a, path) => f(a)
                case e @ JsError(_) => Future.successful(Redirect(routes.messageList.load))
            }    
        }.getOrElse(Future.successful(Redirect(routes.messageList.load)))
    }

    def withSessionUsername(f: String => Future[Result])(implicit request: Request[AnyContent]): Future[Result] = {
        request.session.get("username").map(f).getOrElse(Future.successful(Ok(Json.toJson(Seq.empty[String]))))
    }

    def withSessionUserid(f: Int => Future[Result])(implicit request: Request[AnyContent]): Future[Result] = {
        request.session.get("userid").map(userid => f(userid.toInt)).getOrElse(Future.successful(Ok(Json.toJson(Seq.empty[String]))))
    }

    def validate = Action.async {implicit request =>
        withJsonBody[UserData] { ud =>
            model.validateUser(ud.username, ud.password).map { userExists =>
                userExists match {
                    case Some(userid) => 
                        Ok(Json.toJson(userid))
                            .withSession("username" -> ud.username, "userid" -> userid.toString, "csrfToken" -> play.filters.csrf.CSRF.getToken.get.value)
                    case None => 
                        Ok(Json.toJson(false))    
                }     
            }
        }
    }

    def getGeneralMessages = Action.async { implicit request =>
        withSessionUsername { username =>
            model.getGeneralMessages(username).map(messages => Ok(Json.toJson(messages)))  
        }
    }

    def getPersonalMessages = Action.async { implicit request =>
        withSessionUsername { username =>
            model.getPersonalMessages(username).map(messages => Ok(Json.toJson(messages)))  
        }
    }

    def createUser = Action.async {implicit request =>
        withJsonBody[UserData] { ud => model.createUser(ud.username, ud.password).map { userCreated =>
            if(userCreated) {
                Ok(Json.toJson(true))
            } else {
                Ok(Json.toJson(false))
            }
        }}
    }

    def addGeneralMessage = Action.async { implicit request =>
        withSessionUsername { username =>
            withJsonBody[String] { message =>
                model.addGeneralMessage(message).map(count => Ok(Json.toJson(count > 0)))
            }    
        }
    }

    // def addPersonalMessage = Action.async { implicit request =>
    //     // val receiver = (request.body \ "sender").as[String]
    //     println(receiver)
    //     withSessionUserid { userid =>
    //         val message = request.body.asJson
    //         println(s"$message")
    //         // model.addPersonalMessage(userid, message.toString).map(count => Ok(Json.toJson(count > 0)))
    //         Future.successful(Ok(Json.toJson(true)))
    //     }
    // }

    def addPersonalMessage = Action.async(parse.json) { implicit request =>
        val receiver = (request.body \ "sender").as[String]
        val message = (@@)
        println(receiver)
        withSessionUsername { receiver =>
            model.addPersonalMessage(username, message)    
        }
        Future.successful(Ok(Json.toJson(true)))
        // withSessionUserid { userid =>
        //     val message = request.body.asJson
        //     println(s"$message")
        //     // model.addPersonalMessage(userid, message.toString).map(count => Ok(Json.toJson(count > 0)))
        //     Future.successful(Ok(Json.toJson(true)))
        // }
    }

    def deleteGeneralMessage = TODO
}
```



#### Error stacktrace:

```
scala.collection.LinearSeqOps.apply(LinearSeq.scala:131)
	scala.collection.LinearSeqOps.apply$(LinearSeq.scala:128)
	scala.collection.immutable.List.apply(List.scala:79)
	dotty.tools.dotc.util.Signatures$.countParams(Signatures.scala:501)
	dotty.tools.dotc.util.Signatures$.applyCallInfo(Signatures.scala:186)
	dotty.tools.dotc.util.Signatures$.computeSignatureHelp(Signatures.scala:94)
	dotty.tools.dotc.util.Signatures$.signatureHelp(Signatures.scala:63)
	scala.meta.internal.pc.MetalsSignatures$.signatures(MetalsSignatures.scala:17)
	scala.meta.internal.pc.SignatureHelpProvider$.signatureHelp(SignatureHelpProvider.scala:51)
	scala.meta.internal.pc.ScalaPresentationCompiler.signatureHelp$$anonfun$1(ScalaPresentationCompiler.scala:375)
```
#### Short summary: 

java.lang.IndexOutOfBoundsException: 0