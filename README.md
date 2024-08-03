WeChaty Ping-Pong Bot
This repository contains a simple WeChat bot built using the WeChaty framework. The bot demonstrates handling various message types and events with an object-oriented approach.

Features
Ping-Pong Response: Replies with "pong" and an image when receiving "ping" messages.
Image Handling: Downloads and saves images in different resolutions.
File Handling: Supports receiving and saving audio, video, and attachment files.
Room Management: Lists room members, manages room topics, and handles room member removal.
Friendship Management: Handles new friendship requests and adds new friends.
Alias Management: Gets and sets user aliases.
Installation
bash
Copy code
pip install wechaty wechaty-puppet
Usage
Clone the repository:

bash
Copy code
git clone https://github.com/Ramyakonga/wechaty-ping-pong-bot.git
cd wechaty-ping-pong-bot
Run the bot:

bash
Copy code
python your_bot_file.py
Code Overview
MyBot Class: Extends Wechaty and initializes the bot.
Event Handlers: Handles on_ready, on_message, on_login, on_friendship, and on_room_join events.
Main Function: Starts the bot.
Example Code
python
Copy code
from typing import List, Optional, Union
import asyncio
from datetime import datetime
from wechaty_puppet import get_logger
from wechaty import (
    MessageType,
    FileBox,
    RoomMemberQueryFilter,
    Wechaty,
    Contact,
    Room,
    Message,
    Image,
    MiniProgram,
    Friendship,
    FriendshipType,
    EventReadyPayload
)

logger = get_logger(__name__)

class MyBot(Wechaty):
    def __init__(self) -> None:
        self.login_user: Optional[Contact] = None
        super().__init__()

    async def on_ready(self, payload: EventReadyPayload) -> None:
        logger.info('ready event %s...', payload)

    async def on_message(self, msg: Message) -> None:
        from_contact: Contact = msg.talker()
        text: str = msg.text()
        room: Optional[Room] = msg.room()
        msg_type: MessageType = msg.type()

        if text == 'ping':
            conversation: Union[Room, Contact] = from_contact if room is None else room
            await conversation.ready()
            await conversation.say('pong')
            file_box = FileBox.from_url(
                'https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/'
                'u=1116676390,2305043183&fm=26&gp=0.jpg',
                name='ding-dong.jpg')
            await conversation.say(file_box)

    async def on_login(self, contact: Contact) -> None:
        logger.info('Contact<%s> has logined ...', contact)
        self.login_user = contact

async def main() -> None:
    bot = MyBot()
    await bot.start()

asyncio.run(main())
