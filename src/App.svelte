<script>
  import io from 'socket.io-client'
  let socket = io("http://127.0.0.1:5678")
  import MessageList from './lib/MessageList.svelte'
  import ChatBox from './lib/ChatBox.svelte'

  var messages = [
    "msg 0",
    "msg 1",
    "msg 2",
    "msg 3"
  ]
  var newMessage = ""

  socket.on("msgs", (msgs) => {
    console.log("NEW Message:", msgs);
    messages = msgs
  })

  const sendMessage = () => {
    console.log("sending New Message", newMessage)
    socket.emit("msg", newMessage)
  }
</script>

<main>

<MessageList messages={messages}/>

<ChatBox bind:message={newMessage} on:click={sendMessage}/>

</main>

<style>
</style>
