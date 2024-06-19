#ansys二次开发特定坐标应变提取
coordinate=[]
file="C:/Awork/ansys_ACT/text.txt"
with open (file,"r") as f:
    lines=f.readlines()
    for line in lines:
        coordinate.append([float(x) for x in line.split()])
print len(coordinate)
with Transaction():
    for i in range(long):
        cs=Model.CoordinateSystems.AddCoordinateSystem()
        cs.OriginDefineBy=CoordinateSystemAlignmentType.Fixed
        cs.OriginX=Quantity(coordinate[i][0],"mm")
        cs.OriginY=Quantity(coordinate[i][1],"mm")
        cs.OriginZ=Quantity(coordinate[i][2],"mm")
        analysis=Model.Analyses[0]
        probe=analysis.Solution.AddStrainProbe()
        probe.LocationMethod=LocationDefinitionMethod.CoordinateSystem
        probe.Orientation=cs
        probe.CoordinateSystemSelection=cs
        probe.ResultSelection=ProbeDisplayFilter.Equivalent
        analysis.Solve()
        print probe.MaximumEquivalentStrain.Value, "%.4e" % probe.MaximumEquivalentStrain.Value
