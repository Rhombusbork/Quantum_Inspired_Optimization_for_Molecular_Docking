1. Graph / Topology
The fundamental connectivity — everything else derives from this.

Atom list (element, index)
Bond list (which atoms connected, bond order)
Adjacency matrix
Ring systems (SSSR — Smallest Set of Smallest Rings)


2. Per-Atom Features
AttributeHow DerivedElement / Atomic numberDirect from SMILESFormal chargeDirect from SMILESPartial chargeGasteiger / AM1-BCC algorithmHybridization (sp, sp², sp³)Bond count + π electronsAromaticityHückel's rule on ring systemValenceSum of bond ordersDegreeNumber of bonded neighborsH count (implicit + explicit)Valence rulesIs in ringGraph traversalRing sizeSSSRChirality (R/S)Stereo configuration

3. Per-Bond Features
AttributeHow DerivedBond order (single/double/triple)Direct from SMILESAromaticityRing perceptionIs rotatableNot in ring + not double/tripleStereo (E/Z)Geometric isomerismConjugationAlternating π system detection

4. 3D Geometry
AttributeHow DerivedAtom coordinates (x, y, z)Distance geometry + ETKDG embeddingBond lengthsFrom coordinatesBond anglesFrom coordinatesDihedral / torsion anglesFrom rotatable bondsEnergy-minimized conformationMMFF94 / UFF force field optimization

5. Electrostatics
AttributeHow DerivedPartial atomic chargesGasteiger iteration or AM1-BCCDipole momentSum of charge × position vectorsElectrostatic potential surfaceFrom charges + coordinates

6. Interaction-Relevant Features
These directly determine how the ligand interacts with the receptor:
AttributeHow DerivedH-bond donorsN-H, O-H atomsH-bond acceptorsN, O with lone pairsHydrophobic atomsC, S not adjacent to polar groupsAromatic ringsFrom aromaticity perceptionCharged groupsFormal charge + ionization stateMetal binding sitesHeteroatoms with lone pairs

7. Whole-Molecule Descriptors
AttributeHow DerivedMolecular weightSum of atomic masseslogP (lipophilicity)Wildman-Crippen atom contributionsTPSA (polar surface area)Sum of polar atom surface contributionsRotatable bond countBond-level analysisHBD / HBA countDonor/acceptor countingFormal net chargeSum of formal chargesNumber of ringsSSSR countMolecular formulaAtom counting

8. Fingerprints (optional but useful)
These are useful if your docking pipeline involves any ML scoring:

Morgan / ECFP — circular topology fingerprint
MACCS keys — 166-bit structural key fingerprint
RDKit topological fingerprint