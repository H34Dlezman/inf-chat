<script>
  import io from 'socket.io-client'
  let socket = io(import.meta.env.VITE_SERVER_ADRESS)
  import MessageList from './lib/MessageList.svelte'
  import ChatBox from './lib/ChatBox.svelte'

  var messages = []
  var author = window.localStorage.getItem("author")
  var loaded = false
  var newMessage = ""

  if (author == undefined || author == null) {
    author = ""
    localStorage.setItem("author", author)
  }

  socket.on("msgs", (msgs) => {
    //console.log("NEW Message:", msgs);
    messages = msgs
    loaded = true
    setTimeout(()=>{document.getElementById("MessageList").scroll(0,0)}, 50)
  })

  const sendMessage = () => {
    if (newMessage=="") return
    //console.log("sending New Message", newMessage)

    var message = newMessage

    var msgAuthor = ""
    var authorInd = -1
    if ( (authorInd=message.indexOf("@aka")) >= 0 ) {

      msgAuthor = message.slice(authorInd+4)
      var authorEnd = msgAuthor.indexOf(" ")
      message = message.slice(0, authorInd)

      if (authorEnd>=0 ) {
       message =  message + msgAuthor.slice(authorEnd+1)
       msgAuthor = msgAuthor.slice(0, authorEnd)
      }
      author = msgAuthor
      window.localStorage.setItem("author", author)
    }

    socket.emit("msg", {message, author})
    newMessage = ""
  }
</script>

<main>

<MessageList loaded={loaded} messages={messages}/>

<ChatBox bind:message={newMessage} on:click={sendMessage}/>

</main>

<style>
</style>
