## What is the Community SDK?

[From Wikipedia, the free encyclopedia](https://en.wikipedia.org/wiki/Software_development_kit):

>A software development kit (SDK or devkit) is typically a set of software development tools that allows the creation of applications for a certain software package, software framework, hardware platform, computer system, video game console, operating system, or similar development platform.

The community SDK are scripts, tools and libraries to help you to interact with your Kano devices using your favorite programming language. They are developed to be easy to understand and use in first place.

## Goals
- Enable you to interact with Kano devices/kits on your favorite programming language.
- Enable you to connect your Kano devices with other devices, software or internet services.
- As little dependencies and setup steps as possible.
- Shouldn't require a degree in computer science to understand the SDK code under the hood.
- You should be able to use your favorite tools to write and execute code.

## Not goals
- Performance oriented SDK: If you are looking for a "production" ready SDK, please refer to the Kano Devices SDK (yet to come)
- Use **device features** that are not available on [Kano Code](https://kano.me/app): You can use your favorite programming language to do a lot of things you can't on Kano Code, but your device will not get new features by just using the Community SDK (If you find out we are wrong about that, let us know and you'll be our star!).

## Roadmap
- Python SDK: Connect over Websockets
- Python SDK: Install over `pip`
- Make all the devices features available through the SDKs (only the more likely to be used are available now)
- Add more devices to the SDK: Kickstarter Pixel Kit, Harry Potter wand, light ring (Computer Kit), future kits (speaker kit, camera kit, etc)
- Add other languages: LUA, C#, C++, Java, Ruby, etc
