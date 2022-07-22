<script>
  import io from 'socket.io-client'
  let socket = io(import.meta.env.VITE_SERVER_ADRESS)
  import MessageList from './lib/MessageList.svelte'
  import ChatBox from './lib/ChatBox.svelte'

  var messages = []
  var loaded = false
  var newMessage = ""

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
    var author = ""
    var authorInd = -1
    if ( (authorInd=message.indexOf("@aka")) >= 0 ) {
      author = message.slice(authorInd+4)
      var authorEnd = author.indexOf(" ")
      if (authorEnd>=0 )
       author = author.slice(0, authorEnd)
      message = message.slice(0, authorInd) + author.slice(authorEnd+1)
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
