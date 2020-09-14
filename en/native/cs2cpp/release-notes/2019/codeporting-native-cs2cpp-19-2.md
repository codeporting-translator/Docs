---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 19.2"
linktitle: "CodePorting.Native Cs2Cpp 19.2"
menu:
  docs:
    parent: "2019"
    weight: "2"
lastmod: "2019-10-11"
weight: "2"
---

## Major Features ##

1. Gregorian calendar implementation was revised. Other previously unsupported calendars were implemented.
1. Several bugs were fixed and several features implemented for System::Drawing classes.
1. make_library option was supported in porter.
1. 'includes' attribute was supported for 'implementation' config file node.
1. 'cpp_enum_enable_metadata' option was added making it possible enabling metadata generation for all enums being ported.
1. 'Abort' menu action was fixed in Porter GUI for building step.

## Minor fixes. ##

1. C2487 error was fixed when building ported code with per-class exports for classes implementing ICloneable, IComarable and IEquatable interfaces.
1. Porter behavior was fixed when using 'params'-typed parameter in exception class constructor.
1. A bug was fixed when reading password-protected zip archives.
1. DateTime constructor now throws valid ArgumentOutOfRangeException if it is passed invalid date or time components.
1. BeginPixelProcessing() and EndPixelProcessing() methods were added into Bitmap class allowing it exposing pixels in same premultiplied format as .Net implementation.
1. Graphics::SetClip and Graphics::FillRegion methods were implemented.
1. Some more missing System::Drawing methods were implemented.
1. System::Uri class was improved. Several compilation and runtime issues were fixed. Missing members were implemented, related classes and enums were updated as well.
1. String-argumented constructor behavior was fixed for ObjectDisposedException, ArgumentNullException and ArgumentOutOfRangeException classes.
1. Convinient constructor from char16_t subrange was added into String class.
1. GraphicsPath::Widen method was fixed with Pen dash styles, CompoundArray and thin lines.
1. Image cropping issues after rotating the image were fixed.
1. HotkeyPrefix feature of StringBuilder class was supported.
1. '&#' and '|#' operators were supported for nullable primitives.
1. 'Missing override keyword' warnings of clang compiler were suppressed as porter currently doesn't support auto-placing these.
1. FocusScales feature of PathGradientBrush was supported.
1. Color naming issues were fixed for predefined colors.
1. IsIdentity feature of Matrix class was supported.
1. PixelFormat detection was improved for Bitmap class.
1. Scaling issues with Graphics::DrawImage method were fixed.
1. LinearGradientBrush and PathGradientBrush missing methods were implemented.
1. Several issues with Graphics and Rectangle classes were fixed.
1. Some clipping issues with Graphics class were fixed.
1. Transformed drawing of PathGradientBrush was fixed.
1. Wrong image geometry was fixed for the cases when CompoundArray is used.
1. Some service methods were added into System::Drawing classes to be used by Aspose products only.
1. Graphics::DrawString method was fixed for the case of vertical text. Unexpected NullReferenceException thrown by this method was fixed.
1. BitConverter::GetBytes method was implemented on Linux.
1. Some fonts' behavior was fixed on Linux.
1. Text shadowing was fixed.
1. Line caps were fixed in some cases.
1. GCC compilations issues were fixed with image.h and enum operators.
1. Some kerning issues were fixed.
1. XmlReader::ReadToNextSibling() method was implemented.
1. Nested internal classes forward declaration was fixed.
1. License file was included into Nuget package as per new Nuget.org policy.
1. Shared exports macro was fixed for type conversion operators.
1. Named groups support was fixed for Regex and related classes.
1. Encoding-related classes cloning was fixed.
1. Behaviour of StreamWriter.Writeline(chararray, index, count) method was fixed.
1. Some unwanted binaries were dropped from release package.
1. Compilation issue was fixed with ternary operator applied to boolean variable and item of boolean array.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category
---| ---|  ---|
|PDFCPP-881|Fix error C2487|Bug
|WORDSCPP-710|Rework globalization calendars classes|Task
|PDFCPP-886|CsToCppPorter - parse throw exceptions with params ctor|Bug
|WORDSCPP-750|ZipReaderPal can't extract zip with password / TestAsian.TestJira8745A|Bug
|SLIDESCPP-1480|Fix RegressionTests_v18_10.SLIDESNET_40578 test|Task
|SLIDESCPP-1478|Investigate failed Effects.ImageTransfromOperation.BlurTests functional tests|Task
|SLIDESCPP-1486|Investigate failed Effects.EffectFormat.BlurGrowTests functional tests|Task
|SLIDESCPP-1488|Investigate failed Effects.EffectFormat.SoftEdgesTests functional tests|Task
|SLIDESCPP-1499|Investigate failed Effects.EffectFormat.InnerShadowTests functional tests|Task
|SLIDESCPP-1485|Enhance the implementation of the System::Uri class|Enhancement
|SLIDESCPP-1523|Pen dash styles do not affect GraphicsPath::Widen method|Task
|SLIDESCPP-1524|Image is cropped after applying a transformation matrix|Task
|SLIDESCPP-1525|Build a single library release of Aspose.Slides for C++ by Clang|Task
|SLIDESCPP-1417|Pick fixes for memory leaks and Clang builds from CsPorter repo|Task
|SLIDESCPP-1529|Implement missing drawing methods of the Graphics class|Task
|SLIDESCPP-1531|Implement missing auxiliary methods of the StringFormat class|Task
|SLIDESCPP-1532|Implement missing auxiliary methods of the CustomLineCap class|Task
|SLIDESCPP-1530|Implement missing auxiliary methods of the Graphics class|Task
|SLIDESCPP-1536|Implement System::Drawing::Drawing2D::HatchBrush class|Task
|SLIDESCPP-1537|Resolve Aspose.Slides for C++ v18.11 compilation errors|Task
|SLIDESCPP-1545|Disable compiler warnings for Aspose.Slides for C++ build by Clang|Task
|SLIDESCPP-1544|Implement the FocusScales property of the PathGradientBrush class|Task
|SLIDESCPP-1540|Implement Graphics::MeasureCharacterRanges method|Task
|SLIDESCPP-1549|Fix Color::get_Name method result for predefined colors|Task
|SLIDESCPP-1550|Fix Matrix::get_IsIdentity method behavior|Task
|SLIDESCPP-1552|Fix NotImplementedException while running RegressionTests_v14_10.SLIDESNET_35528 test|Task
|SLIDESCPP-1553|Fix RegressionTests_v18_11.SLIDESNET_37020 test|Task
|SLIDESCPP-1541|Add doxygen documentation to the newly implemented methods|Task
|SLIDESCPP-1534|Implement missing auxiliary methods of the PathGradientBrush class|Task
|SLIDESCPP-1569|Analyze failed BasicTests tests of the Aspose.Metafiles.Tests.Cpp|Task
|SLIDESCPP-1570|Analyze failed BrushTests tests of the Aspose.Metafiles.Tests.Cpp|Task
|SLIDESCPP-1572|Improve clipping algorithm implemented in the Drawing::Graphics class|Task
|SLIDESCPP-1333|Port Aspose.Metafiles.Tests project to C++|Task
|SLIDESCPP-1574|Fix straight lines drawing when CompoundArray is used|Task
|SLIDESCPP-1566|Porter: Improve shared api export mechanism|Task
|SLIDESCPP-1573|Porter: Implement a possibility to hide local symbols in ported code|Task
|SLIDESCPP-1546|Fix symbols export for Aspose.Slides for C++ build by GCC|Task
|SLIDESCPP-1494|Fix RegressionTests_v16_11.SLIDESNET_37864 test|Task
|SLIDESCPP-1598|Incorrect or missing gradient fill|Task
|SLIDESCPP-1599|Incorrect emf geometry rendering|Task
|SLIDESCPP-1528|Distance from the start of a line to the beginning of a dash pattern in C++ is different from .NET|Task
|SLIDESCPP-1600|Missing part of the chart|Task
|SLIDESCPP-1602|Move the ported code of Aspose.Slides.Drawing.Skia.Region to the Aspose C++ Library|Task
|SLIDESCPP-1619|Fix TestDeck_067_StarsBanners.CheckPptx2PdfConversion test|Task
|SLIDESCPP-1609|Vertical text is missing in generated Svg|Task
|SLIDESCPP-1628|Investigate AutoShapeChecksumImport test group issues in Linux|Task
|SLIDESCPP-1630|Investigate tests which fail due to wrong or missing font in Linux|Task
|SLIDESCPP-1640|Fix asposecpplib regression found in tests FontConverterTest|Task
|SLIDESCPP-1641|Fix the product compilation by gcc on Jenkins node 3|Task
|SLIDESCPP-1627|Synchronize Aspose C++ Library sources between Slides and CsPorter repositories (v19.1)|Task
|SLIDESCPP-1642|Merge Aspose C++ Library sources into Slides repository|Task
|SLIDESCPP-1646|Text shadow differs from .NET|Task
|SLIDESCPP-1649|CompoundArray does not affect GraphicsPath::Widen method|Task
|SLIDESCPP-1652|Thin lines outline differs from .NET|Task
|SLIDESCPP-1655|Fix arrows drawing in test SLIDESNET_32304|Task
|SLIDESCPP-1660|Fix Aspose.Slides for C++ compilation errors occurred in Linux|Task
|SLIDESCPP-1661|Investigate a possibility of applying kerning by means of HarfBuzz library|Task
|SLIDESCPP-1664|Merge Aspose C++ Library sources into CsPorter repository|Task
|WORDSCPP-747|Add ability for specifying necessary header files for handmade implemented methods|Task
|PDFCPP-895|commit fix asposecpplib implement XmlReader.ReadToNextSibling (const String& name)|Task
|WORDSCPP-732|Generation of forward declaration for internal inner class does not take into account internal_as_public config option|Task
|CSPORTCPP-2376|licenseUrl node is deprecated|Investigation
|CSPORTCPP-2479|Drop supporting 32-bit versions of products from release.|Task
|PDFCPP-903|Fix CsToCppPorter - ASPOSE_XXX_SHARED_API macros on type conversion operators|Task
|PDFCPP-897|patch boost.regex for using named groups|Task
|WORDSCPP-756|Encoding.Clone() losing information about UTF8Encoding.EncoderShouldEmitUTF8Identifier|Task
|WORDSCPP-720|Adding enum metadata for all enums|Task
|WORDSCPP-757|Incorrect behaviour of StreamWriter.Writeline(chararray, index, count)|Bug
|CSPORTCPP-2485|Fix 'Cancel' command for building step|Task
|CSPORTCPP-2486|Drop gtest binaries if possible|Bug
|WORDSCPP-743|Compilation error in ternary operator with boolean variable and item of boolean array|Task

## Public API and Backward Incompatible Changes ##

1. Some more calendar implementations were added under System::Globalization namespace.
1. Several System::Drawing methods were added an/or implemented.
1. Missing or nonimplemented members of System::Uri class and related classes were implemented.
1. Some missing or nonimplemented members of System::Drawing namespace classes were implemented.
1. HatchBrush class was implemented.
1. Color class was improved.
1. 32-bits VS compilation of ported code is no longer supported.
1. GCC version used to build Linux version of the library was upgraded to 6.5.
