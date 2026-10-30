About this fork / 이 포크에 대하여
=================================

**This is an unofficial fork of OBS Studio. It is not made or supported by
the OBS Project.** It only fixes YouTube bugs that are not fixed upstream.
For the official OBS Studio, go to https://obsproject.com.

**OBS Studio의 비공식 포크입니다. OBS 프로젝트와는 관련이 없습니다.**
공식 버전에서 고쳐지지 않은 YouTube 관련 버그만 수정했습니다.
공식 OBS Studio는 https://obsproject.com 에서 받으세요.

Fixed bugs / 수정한 버그
------------------------

1. **YouTube stream language is always English** (upstream issue
   `#13856 <https://github.com/obsproject/obs-studio/issues/13856>`_)

   When you connect a YouTube account and create a broadcast in OBS
   ("Manage Broadcast"), YouTube always sets the stream language to English,
   because OBS never sends a language. This can also affect auto-generated
   captions. Using a stream key is not affected.

   This fork adds a **Language** box to the YouTube broadcast dialog. It
   defaults to the OBS interface language, and the selection is sent to
   YouTube as ``defaultLanguage`` and ``defaultAudioLanguage``.

   **YouTube 방송 언어가 항상 영어로 설정되는 문제**: OBS에서 YouTube 계정을
   연결해 방송을 만들면("방송 관리"), OBS가 언어 값을 보내지 않아서 방송 언어가
   항상 영어로 잡힙니다. 자동 자막에도 영향을 줄 수 있습니다. 스트림 키
   방식은 해당되지 않습니다. 이 포크는 YouTube 방송 설정 창에 **언어** 선택
   항목을 추가합니다. 기본값은 OBS 인터페이스 언어입니다.

2. **"Auto-stop" setting is not remembered** (same issue #13856)

   With "Remember these settings" on, the Auto-stop checkbox was saved from
   the Auto-start value, so it always came back the same as Auto-start.
   Fixed in ``frontend/dialogs/OBSYoutubeActions.cpp``.

   **"자동 종료" 설정이 저장되지 않는 문제**: "설정 기억하기"를 켜도 자동 종료
   값이 자동 시작 값으로 저장되어, 다시 열면 자동 시작과 같은 값으로
   돌아갑니다.

Building with YouTube support / YouTube 연동 빌드 방법
-----------------------------------------------------

The YouTube account feature needs your own Google OAuth client
(Google Cloud Console → "YouTube Data API v3" → OAuth client, type
"Desktop app"). Without it, YouTube account login is not compiled in.

YouTube 계정 연동을 쓰려면 직접 만든 Google OAuth 클라이언트가 필요합니다
(Google Cloud Console → "YouTube Data API v3" 사용 설정 → OAuth 클라이언트,
유형 "데스크톱 앱"). 없으면 YouTube 계정 로그인 기능이 빌드에서 빠집니다.

Get the source (with submodules) / 소스 받기 (서브모듈 포함)::

   git clone --recursive https://github.com/cloud2519/obs-youtube-language-fix.git

Create ``CMakeUserPresets.json`` in the repository root (it is ignored by git,
never commit your keys) / 저장소 루트에 ``CMakeUserPresets.json`` 을 만드세요
(git에서 제외되는 파일입니다. 키를 커밋하지 마세요):

.. code-block:: json

   {
     "version": 8,
     "configurePresets": [
       {
         "name": "windows-x64-youtube",
         "inherits": ["windows-x64"],
         "cacheVariables": {
           "YOUTUBE_CLIENTID": "<your client id>",
           "YOUTUBE_CLIENTID_HASH": "0",
           "YOUTUBE_SECRET": "<your client secret>",
           "YOUTUBE_SECRET_HASH": "0"
         }
       }
     ],
     "buildPresets": [
       {
         "name": "windows-x64-youtube",
         "configurePreset": "windows-x64-youtube",
         "configuration": "RelWithDebInfo"
       }
     ]
   }

Then build / 그다음 빌드::

   cmake --preset windows-x64-youtube
   cmake --build --preset windows-x64-youtube --parallel

For everything else, follow the official build instructions:
https://github.com/obsproject/obs-studio/wiki/Install-Instructions

----

OBS Studio <https://obsproject.com>
===================================

.. image:: https://github.com/obsproject/obs-studio/actions/workflows/push.yaml/badge.svg?branch=master
   :alt: OBS Studio Build Status - GitHub Actions
   :target: https://github.com/obsproject/obs-studio/actions/workflows/push.yaml?query=branch%3Amaster

.. image:: https://badges.crowdin.net/obs-studio/localized.svg
   :alt: OBS Studio Translation Project Progress
   :target: https://crowdin.com/project/obs-studio

.. image:: https://img.shields.io/discord/348973006581923840.svg?label=&logo=discord&logoColor=ffffff&color=7389D8&labelColor=6A7EC2
   :alt: OBS Studio Discord Server
   :target: https://obsproject.com/discord

What is OBS Studio?
-------------------

OBS Studio is software designed for capturing, compositing, encoding,
recording, and streaming video content, efficiently.

It's distributed under the GNU General Public License v2 (or any later
version) - see the accompanying COPYING file for more details.

Quick Links
-----------

- Website: https://obsproject.com

- Help/Documentation/Guides: https://github.com/obsproject/obs-studio/wiki

- Forums: https://obsproject.com/forum/

- Build Instructions: https://github.com/obsproject/obs-studio/wiki/Install-Instructions

- Developer/API Documentation: https://obsproject.com/docs

- Donating/backing/sponsoring: https://obsproject.com/contribute

- Bug Tracker: https://github.com/obsproject/obs-studio/issues

Contributing
------------

- If you would like to help fund or sponsor the project, you can do so
  via `Patreon <https://www.patreon.com/obsproject>`_, `OpenCollective
  <https://opencollective.com/obsproject>`_, or `PayPal
  <https://www.paypal.me/obsproject>`_.  See our `contribute page
  <https://obsproject.com/contribute>`_ for more information.

- If you wish to contribute code to the project, please make sure to
  read the coding and commit guidelines:
  https://github.com/obsproject/obs-studio/blob/master/CONTRIBUTING.md
  
- Code for the project follows the code style guidelines, located
  here: https://github.com/obsproject/obs-studio/blob/master/CODESTYLE.md

- Developer/API documentation can be found here:
  https://obsproject.com/docs

- If you wish to contribute translations, do not submit pull requests.
  Instead, please use Crowdin.  For more information read this page:
  https://obsproject.com/wiki/How-To-Contribute-Translations-For-OBS

- Contributors to OBS Studio and related repositories are expected to
  follow our Code of Conduct, which can be read here:
  https://github.com/obsproject/obs-studio/blob/master/COC.rst

- Other ways to contribute are by helping people out with support on
  our forums or in our community chat.  Please limit support to topics
  you fully understand -- bad advice is worse than no advice.  When it
  comes to something that you don't fully know or understand, please
  defer to the official help or official channels.


SAST Tools
----------

`PVS-Studio <https://pvs-studio.com/pvs-studio/?utm_source=website&utm_medium=github&utm_campaign=open_source>`_ - static analyzer for C, C++, C#, and Java code.
