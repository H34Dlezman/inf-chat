<script>
  import io from 'socket.io-client'
  let socket = io(import.meta.env.VITE_SERVER_ADRESS)
  import MessageList from './lib/MessageList.svelte'
  import ChatBox from './lib/ChatBox.svelte'

  var messages = []
  var loaded = false
  var newMessage = ""

  socket.on("msgs", (msgs) => {
    console.log("NEW Message:", msgs);
    messages = msgs
    loaded = true
  })

  const sendMessage = () => {
    if (message=="") return
    console.log("sending New Message", newMessage)
    socket.emit("msg", newMessage)
    newMessage = ""
  }
</script>

<main>

<MessageList loaded={loaded} messages={messages}/>

<ChatBox bind:message={newMessage} on:click={sendMessage}/>

</main>

<style>
</style>
