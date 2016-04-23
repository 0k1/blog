---
author: Arun Raj
date: 2015-11-25 12:00:30+00:00
layout: post
slug: creating-pixel-shader-effects-in-wpf
title: 'Creating pixel shader effects in WPF'
---
WPF never fails to show off when it comes to creating awesome visuals and transitions. WPF also allows us to create custom shader effects with HLSL or  the High-Level Shader Language or High-Level Shading Language (HLSL). This is a proprietary shading language developed by Microsoft for the Direct3D 9 API to augment the shader assembly language.HLSL pixel shaders allow you to implement high-quality, hardware rendered visual effects, which enables one to realize the valuable performance advantages of utilizing the computer’s GPU as opposed to the CPU. With the release of WPF 3.5 SP1 developers are now able to develop their own HLSL PS v2.0 compliant shader effects and implement them in WPF applications.

To develop an effect, code the HLSL using an editor of your preference and save the code with a .fx extension. I’m not going to dive deep into that. More  about that [here](https://onedrive.live.com/view.aspx?cid=123ec1ed6c72a14a&page=view&resid=123EC1ED6C72A14A!171&parId=123EC1ED6C72A14A!169&app=Word). Next compile the .fx file into a .ps file using fxc.exe,which comes with the Microsoft DirectX SDK. This can be done via the windows command line. Navigate to the directory containing the .fx file and issue the following command:
`
fxc /T ps_2_0 /E main /Fo<name of HLSL file>.ps <name of HLSL file>.fx
`
Create a project containing all of the custom developed effects and include that project as part of any solution that will utilize them (make sure to remember to add the project as a reference of WPF project that will utilize them also). Set the build task of the .ps file to ‘Resource.’ And last, create a C# wrapper for your effect that sets up dependency properties for any parameters in the HLSL and creates a URI for the .ps file.

If you don’t have DirectX SDK installed, we can compile the fx file with VS as well. Install the Shader Effect Build tasks from [here](http://wpf.codeplex.com/releases/view/14962) . Update the project template to add effect build task. Such templates are already available on the link above.  Then add the fx file with build task as “effect” . This will compile the fx file into a ps file.

I’ll put up an example of MotionBlur pixelshader effect at [github.com/arun619](https://github.com/arun619/MotionBlurShader)

 Cheers,

 Arun