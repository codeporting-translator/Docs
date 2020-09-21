---
date: "2020-06-11"
author:
  display_name: "muhammadrizwan"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 20.6"
linktitle: "CodePorting.Native Cs2Cpp 20.6"
menu:
  docs:
    parent: "2020"
    weight: "4"
lastmod: "2020-06-11"
weight: "4"
---

## Major Features ##

1. The graphic subsystem was reworked significantly. Many rendering bugs were fixed. Image manipulation results are now closer to such in .Net.
1. The Skia version used was updated.
1. More LINQ methods were supported in ported code. This includes Select, OfType, Cast, Contains, Last, LastOrDefault. FirstOrDefault and First methods were fixed.

## Minor fixes ##

1. Boundry checks for System::String::SubString() are now consistent with .Net behavior.
1. Streams and enumerable objects comparison in ported tests is now being done the same way as in NUnit.
1. Retrieval of 64-bit long file sizes was fixed.
1. System::String::LastIndexOf() now supports invariant culture.
1. Exceptions being thrown by the methods of System::String is now consistent with such in .Net.
1. Signatures of System::String::StartsWith() and System::String::EndsWith() methods were put in line with .Net implementations.
1. Performance of System::IO::Path::HasInvalidChars() method was slightly improved.
1. Porter was fixed to work properly with command-line arguments with spaces in them.
1. 'for_each_member_short_names' option was supported by porter. By default, porter now generates for_each_member-related information with long names instead of short ones.
1. has_operator_equal predicate was fixed to compile OK with GCC.
1. LittleCMS dependency was dropped.
1. The translation of the 'string + Nullable' operator was fixed.
1. A potential unreported error issue with XmlTextReader was fixed.
1. Behavior of CompareInfo::Compare(const String& a, const String& b, CompareOptions options) was fixed for some combinations of parameters.
1. Some improvements were made to Doxygen comments.
1. 'Unbox to nullable' behavior was fixed to behave in line with .Net implementation.
1. Assertion ported code for fixed for some cases involving nullable types and/or enums.
1. References to arithmetic types were fixed in Doxygen documentation produced by the porter.
1. XmlTextWriter no longer produces an error if the underlying stream was disposed of.
1. Equals() call was fixed for Nullable types.
1. Porter now processes documentation for public fields.
1. The behavior of the 'nunit_categories' configuration file node was extended, making it possible to include all categories that are not excluded explicitly.
1. 'emit' method of MulticastDelegate class was renamed to 'invoke' to avoid issues when compiling the code in the Qt environment.
1. A compiler warning was fixed coming from System::List's IndexOf(T, int) method.
1. Documentation for System::Boolean type was fixed.
1. Func class is now capable of holding null-reference.

Please consult the respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category
---| ---|  ---|
|EMAILCPP-218|Fix nullptr cast to Nullable|Bug
|SLIDESCPP-1873|Rework Drawing::Pen without using SkLayerRasterizer|Task
|SLIDESCPP-1874|Build asposecpplib using an updated Skia library|Task
|SLIDESCPP-1892|Integrate Skia patches for CMake|Task
|SLIDESCPP-1894|Implement dash caps drawing via SkPathEffect|Task
|SLIDESCPP-1896|Fix errors when compiling fonts|Task
|SLIDESCPP-1897|Fix errors when compiling brushes|Task
|SLIDESCPP-1901|Fix a wrong drawing the custom line cap|Bug
|SLIDESCPP-1910|Adjust build scripts for using Skia with aspose-cpp-libs|Task
|SLIDESCPP-1913|Fix incorrect line filling|Bug
|SLIDESCPP-1924|Fix DashCap drawing for solid dash style lines|Bug
|SLIDESCPP-1930|Fix tests asposecpplib with Clang|Bug
|SLIDESCPP-1931|Fix a wrong scale for line cap|Bug
|SLIDESCPP-1935|Check color blending when using the reworked Drawing::Pen class.|Task
|SLIDESCPP-1939|Fix errors when compile Drawing::Image|Task
|SLIDESCPP-1943|Fix errors when compiling asposecpplib|Task
|SLIDESCPP-1945|Rework simple places in commented code|Task
|SLIDESCPP-1946|Fix a text drawing|Task
|SLIDESCPP-1947|Rework a use of the color palette|Task
|SLIDESCPP-1948|Rework a use of the SkClipStack in the Drawing::Graphics|Task
|SLIDESCPP-1949|Rework a use of the SkTextBox in the Drawing::Graphics|Task
|SLIDESCPP-1959|Rework Drawing::TextureBrush for WrapMode::Clamp|Task
|SLIDESCPP-1960|Rework PathGradientBrush::ApplyPathGradient|Task
|SLIDESCPP-1961|Rework GraphicsPath::AddStringImpl|Task
|SLIDESCPP-1975|Fix GraphicsTest.DrawStringLayoutTest_05 test|Bug
|SLIDESCPP-1976|Fix GraphicsTest.DrawStringLayoutTest_06 test|Bug
|SLIDESCPP-1981|Add UTF16 support to the new SkShaper|Task
|SLIDESCPP-1982|Move the functionality from the old SkShaper to the new one|Task
|SLIDESCPP-1983|Fix memory leak in Drawing::Graphics|Bug
|SLIDESCPP-1985|Fix LinearGradientBrushTest.GammaCorrectionTest|Bug
|SLIDESCPP-1986|Fix PenTest.CompoundArrayTest1|Bug
|SLIDESCPP-1987|Fix PenTest.DashCapTest_01_Flat|Bug
|SLIDESCPP-1988|Fix BGRA color tests for tiff format|Bug
|SLIDESCPP-1989|Fix Pen tests WidthTest_01 and WidthTest_02|Bug
|SLIDESCPP-1990|Fix GifSupportTest.ConvertTest_BGRA_8888|Bug
|SLIDESCPP-1991|Fix GraphicsTest.AntiAliasingTest_01|Bug
|SLIDESCPP-1992|Fix BitmapTest.RotateFlipTest|Bug
|SLIDESCPP-1994|Fix tests when load jpeg with CMYK color scheme|Bug
|SLIDESCPP-1999|Fix TextRenderingHint settings usage when rendering text|Bug
|SLIDESCPP-2000|Fix glyphs positioning in GraphicsPath::AddString calls|Bug
|SLIDESCPP-2002|Merge two implementations of Graphics::DrawString methods into one|Task
|SLIDESCPP-2007|Fix glyphs positioning in Graphics::DrawString calls|Bug
|SLIDESCPP-2022|Implement conversion using a color profile in SkTiffCodec|Task
|SLIDESCPP-2040|Fix GraphicsTest.MeasureDrawString_01 test|Bug
|SLIDESCPP-2049|Fix GifSupportTest.ConvertTest_Index_8 test|Bug
|SLIDESCPP-2050|Fix GifSupportTest.ConvertTest_Index_8_4bit test|Bug
|SLIDESCPP-2057|Fix pixel format definition in Drawing::Bitmap|Task
|SLIDESCPP-2066|Build asposecpplib under Linux|Task
|SLIDESCPP-2069|Fix a palette loss after calling Bitmap::RotateFlip|Bug
|SLIDESCPP-2078|Fix GraphicsTest.MeasureStringTest_10 test|Bug
|SLIDESCPP-2079|Fix DrawStringTests.Test_11 test|Bug
|SLIDESCPP-2080|Fix TiffSupportTest.ColorfullTest_cmyk test that failed on Linux|Bug
|SLIDESCPP-2084|Fix DrawStringTests.Test_08 test|Bug
|SLIDESCPP-2085|Fix DrawStringTests.Test_13 test|Bug
|SLIDESCPP-2126|Update implementation of Bitmap::LockBits/UnlockBits for new Skia|Task
|SLIDESCPP-2127|Update implementation of Graphics::MeasureCharacterRanges for new Skia|Task
|SLIDESCPP-2134|Fix GraphicsTest.MeasureStringTest_07 test|Bug
|SLIDESCPP-2139|Analyze Aspose.Slides for C++ errors built with new Skia|Task
|SLIDESCPP-2144|Fix GifSupportTest.LoadTest1BppGif test|Bug
|SLIDESCPP-2145|Fix BitmapTest.LockBitsWithPixeFormat test|Bug
|SLIDESCPP-2146|Fix TextureBrushTests.AlphaChannelTest test|Bug
|SLIDESCPP-2151|Check currently disabled graphics tests with the new Skia|Task
|SLIDESCPP-2180|Fix symbolic fonts rendering in Linux|Bug
|SLIDESCPP-2184|Configure parameters saving images in PNG format|Task
|SLIDESCPP-2192|Analyze errors in the Aspose.Slides for C++ built with the new Skia (v20.1)|Task
|SLIDESCPP-2222|Fix RegressionTests_v19_8.SLIDESNET_35683 test|Bug
|SLIDESCPP-2224|Fix the calculation of System::Drawing::Font height|Bug
|SLIDESCPP-2225|Fix RegressionTests_v19_2.SLIDESNET_40624 test|Bug
|SLIDESCPP-2230|Fix ArgumentException thrown from the Bitmap constructor|Bug
|SLIDESCPP-2242|Fix text drawing in test RegressionTests_v18_2.SLIDESNET_39697|Bug
|SLIDESCPP-2243|Fix background drawing in test RegressionTests_v19_2.SLIDESNET_34567|Bug
|SLIDESCPP-2256|Fix test BitmapTest.ImageWithErrorInDataFormat_Png in Linux|Bug
|SLIDESCPP-2270|Fix text rendering issues in SLIDESJAVA_33709 test|Bug
|SLIDESCPP-2276|Fix incorrect text clipping|Bug
|SLIDESCPP-2277|Incorrect line spacing|Bug
|SLIDESCPP-2286|Fix SEH exception in Bitmap::set_Palette|Bug
|SLIDESCPP-2291|Fix an issue with saving PNG|Bug
|SLIDESCPP-2320|Сlean code as a result of code review|Task
|SLIDESCPP-2321|Remove lcms2 dependency|Bug
|SLIDESCPP-2322|Fix failed graphics tests on the branch with new Skia in Linux|Bug
|SLIDESCPP-2326|Move the Tiff changes(dpi, compression, pixel format) to new Skia|Task
|SLIDESCPP-2381|Fix performance of the System::Drawing::Region::IsVisible method|Improvement
|SLIDESCPP-2388|Fix an issue with saving 8bpp tiff image|Bug
|SLIDESCPP-2434|Incorrect font in a chart|Bug
|TASKSCPP-1409|Improve nullable +/- ops to avoid "sting + nullable<T>" ambiguity|Bug
|CSPORTCPP-3315|Check release|Task
|SLIDESCPP-2385|Disable failed functional for test FormulaTests_Common.ComparisonOperatorTest|Bug
|TASKSCPP-1414|Implement equality assertions for IEnumerable instances.|Task
|SLIDESCPP-2416|Fix references to POD types in the generated documentation|Bug
|PDFCPP-1297|Generate examples for converting pdf to/from tex, xslfo, etc.|Task
|SLIDESCPP-2103|Porter: Implement documentation comments translation for public fields|Task
|SLIDESCPP-2423|Porter: Make an option to include all non-excluded test categories|Task
|TASKSCPP-1415|Fix a C++ compiler warning on System::List's IndexOf(T, int) method call (casting int64 to int)|Task
|CSPORTCPP-3331|Fix error 404 for System::Boolean documentation|Bug
|WORDSCPP-969|Implement IEnumerable extension methods|Task

## Public API and Backward Incompatible Changes ##

1. Multiple changes were made to rendering results and graphics API.
1. ZipEntry::GetCompressedBytes() method was supported.
1. An overload of MemberInfo::GetCustomAttributes(bool) was supported.
1. StringBuilder::set_Capacity() was implemented.
1. A stub was added for XmlNode::Normalize().
1. A default constructor was added into System::Func class.
