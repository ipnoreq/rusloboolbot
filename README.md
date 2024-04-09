# meta developer: @Mersai

from .. import loader, utils

import random
from contextlib import suppress
from telethon.tl.types import Message, MessageMediaPhoto


bullr = [
    "я тя выибу виувиу?)",
    "я руслан я нюхал пенис и чо)",
    "моя мама на днях сосала пенис и ч)",
    "мне,руслану панкову ,полагается новый дверь на окошко",
    "двигаться попай к пинису этомоеприванье",
    "рекай пока какет руслан)",
    "руслан боженька какашек)",
    "ЦЕ БОТ ДЛЯ САСАНИЯ ПЕНИСА РУСИ)",
]


@loader.tds
class FuckerMod(loader.Module):
    strings = {
        "name": "☠️ Руслотроль"
    }
    
    async def client_ready(self, client, db):
        self.db = db
        self.users = self.db.get("fucker", "users", [])
        self.phrases = self.db.get("fucker", "phrases", [])
    
    def add_phrase(self, phrase: str):
        self.phrases.append(phrase)
        self.db.set("fucker", "phrases", self.phrases)
    
    def add_user(self, user_id: int):
        self.users.append(user_id)
        self.db.set("fucker", "users", self.users)
    
    def remove_user(self, user_id: int):
        self.users.remove(user_id)
        self.db.set("fucker", "users", self.users)
    
    async def clearbullcmd(self, message):
        """Никого не буллить"""
        
        self.users = []
        self.db.set("fucker", "users", self.users)
        
        await utils.answer(
            message=message,
            response="<b>рУСЯ САСЕТ ПЕНИС</b>"
        )
    
    async def bullacmd(self, message):
        """Добавить фразу | .bulla <фраза>"""
        
        args = utils.get_args_raw(message)
        
        if not args:
            return await utils.answer(
                message=message,
                response="<b>🚫 Не указан аргумент</b>"
            )
        
        self.add_phrase(args)
        
        await utils.answer(
            message=message,
            response="<b>Фраза добавлена</b>"
        )
    
    async def bullrcmd(self, message):
        """Вкинуть рандомное оскорбление"""
        
        await utils.answer(
            message=message,
            response=random.choice(bullr + self.phrases)
        )
    
    async def addbullcmd(self, message):
        """Буллить человека. <reply>"""
        
        reply = await message.get_reply_message()
        
        if reply is not None:
            if reply.from_id is not None:
                await utils.answer(
                    message=message,
                    response="<b>☠️ Дай номер своей мамаши бездарь</b>"
                )

                self.add_user(reply.from_id)
            
            else:
                await utils.answer(
                    message=message,
                    response="<b>🚫 Модуль не работает на анонимных администраторах или каналах</b>"
                )

        else:
            await utils.answer(
                message=message,
                response="<b>🚫 Нужен реплай</b>"
            )
    
    async def rmbullcmd(self, message):
        """бУЛЛИТЬ РУСЛАНАа. <reply>"""
        
        reply = await message.get_reply_message()
        
        if reply is not None:
            await utils.answer(
                message=message,
                response="<b>РУСЯ СОСЕТ ПЕНИС</b>"
            )
            
            try:
                self.remove_user(reply.from_id)
            except:
                await utils.answer(
                    message=message,
                    response="<b>💀 РУСЯ СОСЕТ ПЕНИС</b>"
                )

        else:
            await utils.answer(
                message=message,
                response="<b>🚫 Нужен реплай</b>"
            )
    
    async def watcher(self, message):
        if getattr(message, "from_id", None) in self.users:
            await message.reply(random.choice(bullr + self.phrases))
