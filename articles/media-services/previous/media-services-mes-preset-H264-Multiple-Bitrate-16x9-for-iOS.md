---
title: H264 Multiple Bitrate 16 x 9 voor iOS | Microsoft Docs
description: Het onderwerp een overzicht van de **H264 Multiple Bitrate 16 x 9 voor iOS** taak vooraf ingesteld.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 5473e93a-8c18-4fa4-a1b9-215eba325af7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 5f814465da8eeb2261f17e8f0d34fc02437bb5f2
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55988007"
---
# <a name="h264-multiple-bitrate-16x9-for-ios"></a>H264 Multiple Bitrate 16x9 for iOS
`Media Encoder Standard` definieert een reeks voorinstellingen die kunt u bij het maken van coderingstaken coderen. Kunt u ofwel een `preset name` om op te geven in welke indeling u wilt coderen van uw media-bestand. Of, kunt u uw eigen JSON of XML-indeling voorinstellingen (met behulp van UTF-8- of UTF-16-codering. Vervolgens geeft u door de aangepaste voorinstelling voor het coderingsprogramma. Voor een lijst van de vooraf gedefinieerde namen ondersteund door dit `Media Encoder Standard` encoder, Zie [taak voorinstellingen voor Media Encoder Standard](media-services-mes-presets-overview.md).  
  
 In dit onderwerp leest de `H264 Multiple Bitrate 16x9 for iOS` vooraf in XML en JSON-indeling.  
  
 Deze definitie wordt een reeks van 8 GOP uitgelijnde MP4-bestanden, variërend van 8500 kbps tot 200 kbps en stereo AAC-audio. Voor gedetailleerde informatie over het profiel bitrate, steekproeven snelheid, enz. van deze vooraf ingesteld, controleert u de XML of JSON hieronder gedefinieerd. Voor een uitleg van wat elk element in deze middelen voorinstellingen, en de geldige waarden voor elk element, Zie de [Media Encoder Standard schema](media-services-mes-schema.md) onderwerp.  
  
> [!NOTE]
>  Bij het wijzigen van de `Width` en `Height` waarden voor lagen, zorg ervoor dat de hoogte-breedteverhouding consistent blijft. Bijvoorbeeld: 1920x1080, 1280x720, 1080x576, 640x360. U moet een combinatie van verhoudingen, zoals niet gebruiken: 1280x720, 720x480, 640x360.  
  
 XML  
  
```  
<?xml version="1.0" encoding="utf-16"?>  
<Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">  
  <Encoding>  
    <H264Video>  
      <KeyFrameInterval>00:00:03</KeyFrameInterval>  
      <H264Layers>  
        <H264Layer>  
          <Bitrate>8500</Bitrate>  
          <Width>1920</Width>  
          <Height>1080</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>High</Profile>  
          <Level>4</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>8500</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>6500</Bitrate>  
          <Width>1280</Width>  
          <Height>720</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Main</Profile>  
          <Level>3.1</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>6500</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>5000</Bitrate>  
          <Width>1280</Width>  
          <Height>720</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Main</Profile>  
          <Level>3.1</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>5000</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>3500</Bitrate>  
          <Width>960</Width>  
          <Height>540</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Main</Profile>  
          <Level>3.1</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>3500</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>1200</Bitrate>  
          <Width>640</Width>  
          <Height>360</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Baseline</Profile>  
          <Level>3.1</Level>  
          <BFrames>0</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>false</AdaptiveBFrame>  
          <EntropyMode>Cavlc</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>1200</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>600</Bitrate>  
          <Width>640</Width>  
          <Height>360</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Baseline</Profile>  
          <Level>3</Level>  
          <BFrames>0</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>false</AdaptiveBFrame>  
          <EntropyMode>Cavlc</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>600</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>400</Bitrate>  
          <Width>480</Width>  
          <Height>270</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Baseline</Profile>  
          <Level>3</Level>  
          <BFrames>0</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>false</AdaptiveBFrame>  
          <EntropyMode>Cavlc</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>400</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>200</Bitrate>  
          <Width>416</Width>  
          <Height>214</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Baseline</Profile>  
          <Level>3</Level>  
          <BFrames>0</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>false</AdaptiveBFrame>  
          <EntropyMode>Cavlc</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>200</MaxBitrate>  
        </H264Layer>  
      </H264Layers>  
      <Chapters />  
    </H264Video>  
    <AACAudio>  
      <Profile>HEAACV2</Profile>  
      <Channels>2</Channels>  
      <SamplingRate>48000</SamplingRate>  
      <Bitrate>64</Bitrate>  
    </AACAudio>  
  </Encoding>  
  <Outputs>  
    <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">  
      <MP4Format />  
    </Output>  
  </Outputs>  
</Preset>  
```  
  
 JSON  
  
```  
{  
  "Version": 1.0,  
  "Codecs": [  
    {  
      "KeyFrameInterval": "00:00:03",  
      "H264Layers": [  
        {  
          "Profile": "High",  
          "Level": "4",  
          "Bitrate": 8500,  
          "MaxBitrate": 8500,  
          "BufferWindow": "00:00:05",  
          "Width": 1920,  
          "Height": 1080,  
          "BFrames": 3,  
          "ReferenceFrames": 3,  
          "AdaptiveBFrame": true,  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Main",  
          "Level": "3.1",  
          "Bitrate": 6500,  
          "MaxBitrate": 6500,  
          "BufferWindow": "00:00:05",  
          "Width": 1280,  
          "Height": 720,  
          "BFrames": 3,  
          "ReferenceFrames": 3,  
          "AdaptiveBFrame": true,  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Main",  
          "Level": "3.1",  
          "Bitrate": 5000,  
          "MaxBitrate": 5000,  
          "BufferWindow": "00:00:05",  
          "Width": 1280,  
          "Height": 720,  
          "BFrames": 3,  
          "ReferenceFrames": 3,  
          "AdaptiveBFrame": true,  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Main",  
          "Level": "3.1",  
          "Bitrate": 3500,  
          "MaxBitrate": 3500,  
          "BufferWindow": "00:00:05",  
          "Width": 960,  
          "Height": 540,  
          "BFrames": 3,  
          "ReferenceFrames": 3,  
          "AdaptiveBFrame": true,  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Baseline",  
          "Level": "3.1",  
          "Bitrate": 1200,  
          "MaxBitrate": 1200,  
          "BufferWindow": "00:00:05",  
          "Width": 640,  
          "Height": 360,  
          "ReferenceFrames": 3,  
          "EntropyMode": "Cavlc",  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Baseline",  
          "Level": "3",  
          "Bitrate": 600,  
          "MaxBitrate": 600,  
          "BufferWindow": "00:00:05",  
          "Width": 640,  
          "Height": 360,  
          "ReferenceFrames": 3,  
          "EntropyMode": "Cavlc",  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Baseline",  
          "Level": "3",  
          "Bitrate": 400,  
          "MaxBitrate": 400,  
          "BufferWindow": "00:00:05",  
          "Width": 480,  
          "Height": 270,  
          "ReferenceFrames": 3,  
          "EntropyMode": "Cavlc",  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Baseline",  
          "Level": "3",  
          "Bitrate": 200,  
          "MaxBitrate": 200,  
          "BufferWindow": "00:00:05",  
          "Width": 416,  
          "Height": 214,  
          "ReferenceFrames": 3,  
          "EntropyMode": "Cavlc",  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        }  
      ],  
      "Type": "H264Video"  
    },  
    {  
      "Profile": "HEAACV2",  
      "Channels": 2,  
      "SamplingRate": 48000,  
      "Bitrate": 64,  
      "Type": "AACAudio"  
    }  
  ],  
  "Outputs": [  
    {  
      "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",  
      "Format": {  
        "Type": "MP4Format"  
      }  
    }  
  ]  
}  
```
