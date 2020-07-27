# DiscordJS Bot Starter
Simple class based DiscordJS bot starter with two example commands.

## Commands
- `!ping` : Returns gateway ping
- `!announce` : Sends a message in a given channel

## How it works?
Each command is based in a class. To create a new command you just need to create it's corresponding class and enjoy. 🎉

## How to start?
1. Clone the project
2. Run `yarn`
3. Create a `.env` file and paste & replace the content of `.env.example`
4. Run `yarn start:watch` for development.

### How to add a new command?
Create the corresponding command file in the `/commands` directory and follow the example structure. The bot will automatically take all the commands exported from `/commands/index.ts`.

### How to structure a command class?
This is the basic structure of a command class :
```ts
class HelloCommand extends Command {
  
  /**
   * The constructor will basically receive all the command metadata, what the command should do etc... It's the command configuration !
   **/
  constructor(message: Message) {
    super(message, {
      command: "hello", // this is to what the bot should react to.
      args: [ // here you add an array arguments (optional).
        {
          key: "channel", // the internal argument key
          description: "The channel where the hello should be sent.",
          required: true // if the argument is required or not
        },
      ],
      description: "Says \"hello\" in a specified channel." 
    })
  }

  /**
   * This is the main command handler, this is the method that will be called when the command is executed.
   **/
  handler = async () => {
    // Get the channel object from the command arguments
    const channel: TextChannel = <TextChannel>this.message.guild.channels.resolve(this.argument("channel").value.match(/[0-9]+/g)[0]);

    // The class has two important methods : message & argument
    // this.message : returns the message object that hired the command
    // this.argument("argument key") : returns an argument by it's key

    // You can throw errors that will be returned to the used !
    if (!channel) throw new Error("Unrecognized channel !");

    // Delete the message that hired the command
    await this.message.delete();
    
    // Send the Hello World message
    await channel.send("Hello world !");

    // Feedback "Done !" to the user and delete the message after 1,5 seconds  
    await this.message.channel.send("Done !").then(msg => msg.delete({ timeout: 1500 }));
  }
}
```
Please see the two examples in the starter project, and feel free to contact me (open an issue?) for any questions ! 😊