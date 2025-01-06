
@RestController
public class MessageController {

    @MessageMapping("/message")
    @SendTo("/topic/return-to")
    public Message getContent(@RequestBody Message message) {

        try {
            Thread.sleep(500);

        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return message;
    }

}
