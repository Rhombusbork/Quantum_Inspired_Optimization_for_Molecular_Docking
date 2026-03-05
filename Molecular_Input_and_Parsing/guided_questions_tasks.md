# LIGAND LOADING MODULE: Steps 1.1.1 - 1.1.3
## Guided Development Tasks

---

## TASK 1: INPUT PARSING (Step 1.1.1)

### Context
In molecular docking, ligands can be represented in multiple formats:
- **SMILES**: Text-based chemical notation (e.g., "CCO" for ethanol)
- **InChI**: International Chemical Identifier (standardized representation)
- **MOL/SDF files**: 3D structure files containing atom coordinates

### Your Challenge
Design a system that can accept molecular input in different formats and convert them into a unified internal representation (molecule object).

### Questions to Guide Your Design:

**Q1.1**: What data structure would you use to represent a "molecule object" that needs to store:
- A list of atoms (with element types)
- A list of bonds between atoms
- Chemical properties (aromaticity, charge)

**Q1.2**: When parsing a SMILES string like "c1ccccc1" (benzene), what validation checks should you perform before accepting it as valid input?
- What happens if the string has invalid characters?
- What if the chemical structure is impossible (e.g., carbon with 5 bonds)?

**Q1.3**: Design a function `load_from_smiles(smiles_string)` that:
- Takes a SMILES string as input
- Returns a molecule object if successful
- Returns NULL/None if parsing fails
- What should this function do? Write out the steps.

**Q1.4**: How would you handle multiple input formats (SMILES, InChI, MOL files) without duplicating code?
- Should you have separate functions for each format?
- What common logic can be shared?

**Q1.5**: Error handling - if parsing fails, what information should you provide to the user?
- Just return NULL/None?
- Store error messages?
- Throw exceptions?

---

## TASK 2: MOLECULE SANITIZATION (Step 1.1.2)

### Context
After parsing, molecules need "sanitization" - a validation process that:
- Checks if atoms have valid valences (correct number of bonds)
- Identifies aromatic rings (benzene, pyridine, etc.)
- Assigns single/double bond patterns to aromatic systems
- Detects chemically impossible structures

### Your Challenge
Implement a validation system that ensures the molecule is chemically valid before proceeding.

### Questions to Guide Your Design:

**Q2.1**: What does "valence checking" mean, and why is it necessary?
- Example: Carbon typically forms 4 bonds. What should happen if you encounter C with 5 bonds?
- How would you detect this programmatically?

**Q2.2**: Aromatic rings (like benzene) are special. They have:
- Alternating single/double bonds that resonate
- Specific electron configurations (Hückel's rule: 4n+2 π electrons)

Design a way to:
- Identify which atoms are part of aromatic rings
- Mark bonds as "aromatic" vs. "single" vs. "double"

**Q2.3**: Design a function `sanitize_molecule(mol)` that:
- Takes a molecule object as input
- Performs all validation checks
- Returns True if valid, False if invalid
- What steps would this function execute? List them in order.

**Q2.4**: What should happen when sanitization fails?
- Example: User provides "C(=O)=O" (carbon with two double bonds to oxygen - impossible)
- Should you:
  - Reject the molecule?
  - Try to fix it automatically?
  - Warn the user but continue?

**Q2.5**: Some sanitization tasks include:
- Valence checking
- Aromaticity perception
- Kekulization (assigning explicit single/double bonds)
- Implicit hydrogen counting
- Ring detection

Which of these must be done first? Is there a required order? Draw a dependency diagram.

---

## TASK 3: EXPLICIT HYDROGEN ADDITION (Step 1.1.3)

### Context
Chemical notations often omit hydrogens for brevity:
- "C" in SMILES actually means CH₄ (methane - carbon with 4 hydrogens)
- "CCO" means CH₃CH₂OH (ethanol)

For computational analysis, we need to add all hydrogen atoms explicitly.

### Your Challenge
Convert a molecule with implicit hydrogens into one with all hydrogens explicitly represented.

### Questions to Guide Your Design:

**Q3.1**: Why do we need explicit hydrogens for molecular docking?
- Consider: hydrogen bonding, electrostatic interactions, molecular volume
- What calculations would fail without explicit hydrogens?

**Q3.2**: Given a carbon atom with:
- Atomic number: 6
- Current bonds: 2 single bonds to other carbons
- Formal charge: 0

How many hydrogens should you add? Walk through the logic.

**Q3.3**: Design a function `add_hydrogens(mol)` that:
- Takes a molecule object with implicit hydrogens
- Adds explicit hydrogen atoms
- Returns the modified molecule
- What steps does this function need to perform?

**Q3.4**: After adding hydrogens:
- How does the atom count change?
- How does the bond count change?
- Should you track this information?

**Q3.5**: Consider the molecule "c1ccccc1" (benzene):
- Before adding hydrogens: 6 atoms (all carbons)
- After adding hydrogens: ? atoms
- How many hydrogens are added to each carbon?
- Write out the explicit structure.

---

## TASK 4: PIPELINE INTEGRATION

### Context
Steps 1.1.1, 1.1.2, and 1.1.3 must work together as a pipeline. Each step depends on the previous step succeeding.

### Your Challenge
Design a complete pipeline that orchestrates all three steps with proper error handling and data flow.

### Questions to Guide Your Design:

**Q4.1**: Design a function `pipeline_steps_1_to_3(input_string, input_type)` that:
- Executes all three steps in sequence
- Stops if any step fails
- Returns the final processed molecule

What should the control flow look like? (Use if/else, try/catch, etc.)

**Q4.2**: What happens if:
- Step 1.1.1 (parsing) succeeds
- Step 1.1.2 (sanitization) fails

Should you:
- Continue to step 1.1.3 anyway?
- Return immediately?
- Try to fix the issue?

**Q4.3**: How would you provide feedback to the user during execution?
- Silent execution until the end?
- Print status messages after each step?
- Progress bar?
- Log file?

**Q4.4**: Design an error tracking system. If multiple steps fail, how do you:
- Store all error messages?
- Associate errors with specific steps?
- Present errors to the user?

**Q4.5**: Should your pipeline be:
- **Procedural**: Three separate functions called in sequence
- **Object-oriented**: A class that maintains state between steps
- **Functional**: Pure functions with no side effects

Justify your choice.

---

## TASK 5: DATA STRUCTURE DESIGN

### Context
Throughout all three steps, you're building and modifying a "molecule object." This object must store chemical information efficiently.

### Your Challenge
Design the core data structures for representing molecules.

### Questions to Guide Your Design:

**Q5.1**: A molecule consists of atoms. For each atom, you need to store:
- Element type (C, N, O, etc.)
- Position in 3D space (x, y, z coordinates)
- Formal charge
- Number of implicit hydrogens
- Is it aromatic?

Design a data structure to represent ONE atom.

**Q5.2**: A molecule also has bonds between atoms. For each bond, you need:
- Which two atoms are connected?
- Bond type (single, double, triple, aromatic)
- Stereochemistry (cis/trans, E/Z)

Design a data structure to represent ONE bond.

**Q5.3**: Now design a "Molecule" data structure that contains:
- A collection of atoms
- A collection of bonds
- Molecular properties (total charge, molecular weight)

How would you organize this?

**Q5.4**: Consider these operations you'll need to perform:
- "Find all bonds connected to atom #5"
- "Check if atoms #3 and #7 are bonded"
- "Get all aromatic atoms"

What data structure makes these operations efficient?
- List of atoms + list of bonds?
- Adjacency matrix?
- Adjacency list?
- Graph with atoms as nodes?

**Q5.5**: After adding hydrogens in step 1.1.3, the atom list grows. How do you:
- Insert new hydrogen atoms into your data structure?
- Update bond lists to include C-H bonds?
- Maintain atom indices/IDs?

---

## TASK 6: TESTING & VALIDATION

### Context
Your code must handle both valid molecules and invalid inputs gracefully.

### Your Challenge
Design test cases to ensure your implementation works correctly.

### Questions to Guide Your Testing:

**Q6.1**: Create 5 test molecules with different characteristics:
- A simple linear molecule (e.g., ethanol)
- An aromatic molecule (e.g., benzene)
- A molecule with multiple rings (e.g., naphthalene)
- A charged molecule
- An invalid molecule (should fail sanitization)

For each, what's the SMILES string?

**Q6.2**: For each test molecule, manually calculate:
- Number of atoms before adding hydrogens
- Number of atoms after adding hydrogens
- Number of bonds before and after
- Which atoms are aromatic?

**Q6.3**: Design test cases for error conditions:
- What should happen with an empty SMILES string?
- What about invalid characters?
- What about a chemically impossible structure?

**Q6.4**: How would you verify that sanitization worked correctly?
- What properties should you check?
- How do you know aromaticity was assigned correctly?

**Q6.5**: Create a test for the complete pipeline:
- Input: "CCO" (ethanol)
- Expected output: A molecule object with:
  - ? atoms (calculate this)
  - ? bonds (calculate this)
  - Specific atom types in specific positions

Write out the expected values.

---

## TASK 7: OPTIMIZATION & DESIGN CHOICES

### Context
There are multiple ways to implement this system. Consider trade-offs between simplicity, performance, and maintainability.

### Questions to Explore:

**Q7.1**: Should each step (1.1.1, 1.1.2, 1.1.3) be:
- A standalone function?
- A method in a class?
- A separate module/file?

What are the pros/cons of each approach?

**Q7.2**: Error handling strategy:
- **Option A**: Return None/NULL on failure, require checking after each step
- **Option B**: Throw exceptions, use try/catch blocks
- **Option C**: Return status codes + error messages

Which approach is best for this pipeline? Why?

**Q7.3**: State management:
- Should the molecule object be immutable (each step returns a new object)?
- Or mutable (each step modifies the object in place)?

What are the implications for debugging and testing?

**Q7.4**: If you need to process 10,000 molecules in a batch:
- How would you modify your pipeline?
- What optimizations would you make?
- How would you parallelize the work?

**Q7.5**: Logging and debugging:
- What information should be logged at each step?
- How verbose should logging be?
- Where should log messages go (console, file, both)?

---

## DELIVERABLE

**Your task**: Using your answers to the questions above, write pseudocode that implements Steps 1.1.1 - 1.1.3 of the ligand loading module.

Your pseudocode should include:

1. **Data structures** for representing molecules, atoms, and bonds
2. **Function signatures** for all three steps
3. **Control flow** showing how steps connect in a pipeline
4. **Error handling** logic
5. **Comments** explaining key decisions

**Format**: Write your pseudocode in a clear, structured format similar to Python or C++, but without worrying about exact syntax. Focus on logic and structure.

**Example format**:
```
FUNCTION parse_smiles(smiles_string):
    INPUT: smiles_string (String)
    OUTPUT: molecule object OR null
    
    STEP 1: Validate input string
        IF string is empty:
            RETURN null
    
    STEP 2: Parse characters
        FOR each character in smiles_string:
            ...
    
    STEP 3: Build molecule object
        ...
    
    RETURN molecule
```

**Evaluation criteria**:
- Does your pseudocode cover all three steps?
- Does it handle errors appropriately?
- Is the data flow clear?
- Would a programmer be able to implement real code from your pseudocode?

---

## BONUS CHALLENGES

Once you've completed the basic pseudocode:

**B1**: Extend your design to handle multiple input formats (SMILES, InChI, MOL files) with a unified interface.

**B2**: Add a validation step that checks for common mistakes:
- Disconnected fragments (e.g., "CCO.C" - ethanol + methane)
- Radicals (atoms with unpaired electrons)
- Unusual charges

**B3**: Design a caching mechanism so that if the same SMILES string is parsed twice, you reuse the result instead of reprocessing.

**B4**: Add 3D coordinate generation for the hydrogen atoms added in step 1.1.3 (hint: consider bond angles and geometry).

**B5**: Create a "molecule fingerprint" after step 1.1.3 that can be used to quickly compare molecular similarity (preview of step 1.1.8).

---

## RESOURCES & HINTS

**Hint for Q1.1**: Think about how you'd represent a graph in computer science. Molecules ARE graphs!

**Hint for Q2.2**: Aromatic rings are cyclic. What graph algorithm finds cycles?

**Hint for Q3.2**: Valence formula: `hydrogens_to_add = typical_valence - current_bonds - formal_charge`

**Hint for Q4.2**: In a pipeline, one failure should stop the entire process. This is called "fail-fast."

**Hint for Q5.4**: Adjacency list vs. adjacency matrix - consider: sparse vs. dense graphs, lookup speed, memory usage.

**Think about**: 
- What happens when molecules have thousands of atoms?
- How would you make your code reusable for steps 1.1.4 - 1.1.12?
- What would a "Molecule" class API look like?

Good luck! 🧪🔬
