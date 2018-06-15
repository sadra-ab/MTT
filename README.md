# C# DTOs to Typescript Interfaces

MTT generates TypeScript interfaces from .NET DTOs. It implements most major features of the current TypeScript specification. This utility could be preferred over some others as it is completely independent of your IDE or workflow, because it uses a MSBUILD task and converts the code directly from the source. This means its great for .Net Core and VS Code.

## Install

Using dotnet CLI:

`dotnet add package MTT`

`dotnet restore`

Then in .csproj add a Target.

## Options

_WorkingDirectory_ is the input directory of the c# dtos

_ConvertDirectory_ is the output directory of the ts interfaces

_AutoGeneratedTag_ (default true) show "/_ Auto Generated _/" at the top of every file

## Example

#### .csproj

```
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="MTT" Version="0.4.4"/>
  </ItemGroup>

  <Target Name="Convert" BeforeTargets="PrepareForBuild">
    <ConvertMain WorkingDirectory="Resources/" ConvertDirectory="models/"/>
  </Target>

</Project>
```

#### Vehicle.cs

```
using System.Collections.Generic;
using Example.Resources.Parts;

namespace Example.Resources.Vehicles
{
    public class Vehicle : Entity
    {
        public VehicleMake Make { get; set; }
        public VehicleModel Model { get; set; }
        public VehicleYear Year { get; set; }
        public VehicleState Condition { get; set; }  //this is an enum<int>
        public string Description { get; set; }
        public ICollection<VehiclePart> Parts { get; set; }
    }
}
```

#### vehicle.ts

```
/* Auto Generated */

import { Entity } from "../entity"
import { VehicleMake } from "./vehicleMake"
import { VehicleModel } from "./vehicleModel"
import { VehicleYear } from "./vehicleYear"
import { VehicleState } from "./vehicleState"
import { VehiclePart } from "../Parts/vehiclePart"

export interface Vehicle extends Entity {
	make: VehicleMake;
	model: VehicleModel;
	year: VehicleYear;
	condition: VehicleState;
	description: string;
	parts: VehiclePart[];
}
```

## Types

It correctly converts the following C# types to the equivalent typescript:

*   bool
*   byte
*   decimal
*   double
*   float
*   int
*   uint
*   long
*   sbyte
*   short
*   string
*   ulong
*   ushort
*   Boolean
*   Byte
*   Char
*   DateTime
*   Decimal
*   Double
*   Int16
*   Int32
*   Int64
*   SByte
*   UInt16
*   UInt32
*   UInt64
*   Array
*   Collection
*   Enumerbale
*   IEnumerable
*   ICollection
*   Enum

## Notes

**If a _Convert Directory_ is supplied, it will be deleted everytime script is ran and will be remade**

Comments like `//` are ignored in c# files.  Comments like `/* */` could cause undefined behavior.

Matches the directory structure of the dto's, however it only checks 1 lower directory from Working Directory

Since javascript does not have an enum, but typescript does, only enums of type int will be supported for simplicity.

Read more about typescript enums [here](https://www.typescriptlang.org/docs/handbook/enums.html)

Tested on Windows 10 and macOS High Sierra

Follows the case and naming conventions of each language.

Thanks to natemcmaster [this project](https://github.com/natemcmaster/msbuild-tasks) really helped me out!
