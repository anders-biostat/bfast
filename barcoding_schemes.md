## Barcode Strategies for general LAMP project and B-FAST LAMP project


## General LAMP Project, current approach

Every test kit contains a tube and a paper slip, both with the same bar code. 

Printing procedure:

- From a pre-generated list of 100,000 random 6-letter codes, a batch of 50 codes is taken

- For these, codes are printed on labels in doublets: one label for the tube, one for the slip.

- 50 kits form a batch, packed into a bag.


## General LAMP Project, future approach

### for tubes without Micronics-style barcode

Each tube gets a barcode label with a code of the form "ZLnnnnnn", where "nnnnnn" is a consecutively increasing number, padded with leading zeroes to 6 digits. It is packed together with a paper slip with a numeric "access code" of the form "kkk kkk kkk", which is generated randomly and tied to the tube barcode. 50 such kits form a "bag" (no longer "batch" to avoid confusion with test batches)

#### Part A: Labelling of tubes

- A range of values for nnnnnn is chosen, typically such that the range starts with a multiple of 50 and ends with a multiple of 50 minus 1.

- Tube codes are grouped into bags: For example, bag Z23A contains codes from ZL002300 to ZL002349 and bag Z23B contains codes from ZL002350 to ZL002399, i.e.: the bag IDs always start with "Z" (for "ZMBH") followed by the four leading digits (with leading zeroes omitted), and then A if the final two digits are 0 to 49 or B if they are 50 to 99.

- Now, a roll of labels is printed. Each batch starts with a full-width dummy label like "batch Z23A" followed by 25 labels, each containg two of the codes. To do so, we will have a script that takes a starting value and a number of batches to be printed, and prints the labels.

- The labels from a batch are printed on 50 tubes. These are placed in a bag labelled with the batch label.

#### Part B: Assembling the kits

A bag with 50 labelled tubes is taken. For each tube, a person performs the following activity:

- Take a tube

- Hold it against the scanner

- The label printer prints out a paper slip.

- The paper slip is folded so that its content cannot be seen from outside.

- The tube and the paper slip are put into plastic bag, which already contains the other items (salt water flask, straw)

The 50 bags thus filled are placed in a big lab, which is labeleld with the bag number using a marker

#### The scanner-printer action

Whenever the worker holds a tube against the scanner, the following is done by the computer:

- The tube barcode is read.

- The data base (containing assignments of tube barcodes to access codes) is consulted to ensure that this tube barcode has not yet been assigned a 9-digit access code.

- A 9-digit access code is randomly generated. The data base is consulted to ensure that this code has not been generated before.

- The assignment of the newly generated access code to the tube bar code is stored in the data base.

- A paper slip is printed, containing

  - a general information text

  - the URL https://covidtest-heidelberg.de

  - the access code in a box

  - a QR code, linking to https://covidtest-heidelberg.de/sample?access=kkkkkkkkk


### for Micronics tubes with barcode

Same procedure as in previous section, but Part A is omitted. Instead, a batch is formed by simply taking 50 tubes from a Micronics shipment. (Or, perhaps, we should have only 48 tubes per batch so that we can use exactly half a plate to form a bag.)

The bag IDs now get the form "Ynnnn", where "Y" means that it is a bag of Micronics tubes and "nnnn" is a consecutive numbering.


## B-FAST project, Arm A1

Each shipment contains 

- a cover letter ("Anschreiben") 
- a consent form ("Einwilligung")
- a health questionnaire
- a brochure
- a test kit, containg: a tube with barcode, a flask with saline, a straw
- a return envelope and a sample bag with absorbant inlay.


### Part A: Assembling the test kits

- A sufficient quantity of labels are printed, with barcodes of the style "BFnnnnnn" and sticked on tubes. Alternative, a quantity of pre-barcoded Micronics tubes can be used.

- Plastic bags are filled with a tube, a saline flask, and a straw. It is important that the barcode can be easily read through the plastic bag.


### Part B: Printing of cover letter and consent form

- Using the register data, a table of subjects with addresses is generated

- For each subject, a randomly generated access code (9 digits) is added.

- The access codes are added to the data base, preliminarily assigned to the dummy tube code `__reserved__`. When adding them, we check that they are not yet assigned.

- The table is used to print cover letter, consent form and questionnaire

- The cover letter contains that access code as digits (in big font) at the top right and as a barcode within the address area (such that it can be read through the envelope window). The consent form and the questionnaire contain the barcode at some fixed place.

- The cover letter may also contain a QR code with a URL for the result query page, including the access code.

- The printout is collated such that the cover letters, consent forms and questionnaires alternate, with all material for one addressee in one bunch.

- The papers for a given subject are put into a C4 envelope with window. The age-appropriate extra material is also added.

- The brochure is added, as well as the return envelope and the sample-return bag.


### Part C: Merging 

A worker 

- takes a pre-filled envelope (from part B)

- scans the bar code with the access code through the envelope window

- takes a plastic bag (from part A)

- scans the tube bar code through the plastic bag

- puts the bag into the envelope

- closes the envelope


### Scanner activity

During part C, the following happens on the IT side

- When the access code is scanned, the script verifies that this access code is assigned to `__reserved__`.

- When the tube is scanned, the script verifies that the tube code is not yet assigned to an access code.

- The script changes the access code's assignment to the tube code.

- The script emits a pleasant bell sound to indicate that the match is stored


## B-FAST project, arm A2

### Part A: Assembling the test kits

- A sufficient quantity of labels are printed, with barcodes of the style "BGnnnnnk". There are always groups of four barcodes, with the same nnnnn but k ranging from 1 to 4. Each label contains the barcode as usual Code-128 with clear text below, but also, at the right hand side of the label, a large single digit in a circle, which indicates k.

- If we use wide labels, we always put the four labels below each other, so that each label contains bar codes from two groups.

- A worker takes the four labels from a group, sticks them to four tubes, and puts these four tubes into one plastic bag.

- Four saline flasks and four straws are added to the bag.

Note: This procedure is not suitable for Micronics tubes. One could devise an alternative procedure where one takes four pre-barcoded tubes, sticks small stickers with "1" to "4" to them, then scans them in order. This seems to be more error-prone, however.


### Part B: Preparing the documents.

- As in A1, we generate a table from the register data. We assume here that all the register data already has an ID key labelling address records.

- Now, we add four randomly generated access codes to each address, i.e., we have four new columns

- All four are added to the data base as `__unassigned__`, with checking that they have not been issued yet.

- The cover letter now lists four 9-digit access codes, each preceded with a digit (1 to 4) in a circle.

- The consent forms have space for four people, with spaces clearly labelled with 1 to 4 in a circle. 

- Cover letter and consent form have bar codes, as before. These are now the access codes of number 1 only. 

- Questionnaires are provided in quadruples, with each form being clearly labelled with a digit 1 to 4 in a circle, and the corresponding access code as barcode somewhere.

- Everything is printed and collated as before.

- Also as before, documents are placed in an envelope.


### Part C: Merging

A worker 

- takes a pre-filled envelope (from part A)

- scans the barcode visible in the envelope's window

- takes a bag with four tubes

- scans the barcode on one of the tubes (does not matter which one)

- places the bag into the envelope and clsoes the envelope


### IT side

While scanning, the following happens internally

- A barcode from an envelope is scanned

- The script searches for this barcode in the table of register data, looking in the first of the four access code columns.

- The script retrieves all four access codes for this address record.

- It checks that these access codes are all mapped to `__unassigned__`

- Then, a tube barcode is scanned.

- The script verifies that it starts with "BG" and ends with [1-4].

- The script reconstructs all four tube barcodes by replacing the last digit of the scanned bar code with the digits 1 to 4.

- The script verifies that none of these bar codes are assigned to any access codes yet.

- It then assignes the four tube bar codes to the four access codes.

- It emits a bell sound to indicate success.


### B-FAST project: Arm A2, extra tubes

If a subject from the A2 arm calls the hotline to ask for additional tubes, the hot line writes down the subject's access number.

Note: 3% of German households have more than 4 members (my estimate from a graph with 2018 data). Therefore, we expect this to happen less than 200 times. So, we do not need an overly optimized procedure.

Preparation: We prepare a list of further barcode rolls with type "BGnnnnnk", as above, with k going from 5 to 9, and a few more with "BHnnnnkk" with kk from 05 to 20 or so.

#### Daily procedure:

- We get a list of access numbers from the hot line, together with numbers of requested tubes.

- For each access number, we retrieve the address. We add the desired number of access codes. 

- We print cover letters with the access codes, followed by consent forms with blocks labelled 5 to 9. They all contain the access code for 5 as barcode. 

- These are put in envelopes.

- Taking an envelope, we take the required number of barcode labels, discard the remaining ones, add them to tubes, put them in a bag, together with saline and straws.

- Then scan the cover letter barcode and one of the tubes and put the tubes into the envelope. Seal the envelope.



### B-FAST project: Arm B1

#### Sending out invitation letters

The invitation letters contain an access code, the URL to the questionnaire, and a QR code linking to the questionnaire, with data pre-filled in.

#### Sending out test kits

After analysing the questionnaire responses, a table of addresses is generated. We can then proceed as in Arm A1.


### B-FAST project: Arm B1

#### Sending out invitation letters

This time, the invitation letters ask the addressee to also ask the other household members to fill out the forms. The web form will allow the questionnaire to be filled out for multiple people, always asking for their name and age.
