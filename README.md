import React, { useState, useRef, useEffect } from 'react';
import { Container, Row, Col, Collapse, Button } from 'react-bootstrap';
import axios from 'axios';
import Image from 'next/image';
import LoginModule from '../../styles/login.module.css';
import WebappsModule from '../../styles/webapp.module.css';
import FormModule from '../../styles/form.module.css';
import { faUserLarge, faChevronRight, faChevronDown, faX, faPen, faTrashAlt, faChevronLeft } from '@fortawesome/free-solid-svg-icons';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import Close from "../../../public/commonIcons/closeGray.png";
import Checkboxes from '../../../public/commonIcons/Checkboxes.png';
import Copy from '../../../public/formbuilder/copy.png';
import Fileupload from '../../../public/formbuilder/Fileupload.png';
import Calendar from '../../../public/commonIcons/Calendar.png';
import ReactDOM from 'react-dom';
import { fetchAPI, getAuth, getUser, getBaseUrl, getHeaders } from '../../pages/api';
import { get } from 'react-hook-form';
import Link from 'next/link';
import Tooltip from 'react-bootstrap/Tooltip';
import OverlayTrigger from 'react-bootstrap/OverlayTrigger';

    function FormBuilder() {

        const [open, setOpen] = useState(false);
        const arrowIcon = open ? faChevronDown : faChevronRight;
        const [isOpen, setIsOpen] = useState(true);
        // this we declared for collapse 
        const [openCollapse1, setOpenCollapse1] = useState(false);
        const [openCollapse2, setOpenCollapse2] = useState(false);
        const [openCollapse3, setOpenCollapse3] = useState(false);
        const [openCollapse4, setOpenCollapse4] = useState(false);
        const [openCollapse5, setOpenCollapse5] = useState(false);
        const [openCollapse6, setOpenCollapse6] = useState(false);
        // brand details 
        const [drugName, setDrugname] = useState('');
        const [indication, setIndication] = useState('');
        const [pageTitle, setPageTitle] = useState('');
        const [instructions, setInstructions] = useState('');
        const [pharmaid, setPharmaid] = useState('');
        const [formName, setFormName] = useState('');
        const [formFields, setFormFields] = useState([]);
        const [isFormEmpty, setIsFormEmpty] = useState(true);
        const containerRef = useRef(null);
        const [manufacturerChecked, setManufacturerChecked] = useState(false);
        const [brandChecked, setBrandChecked] = useState(false);
        const [programChecked, setProgramChecked] = useState(false);
        const [consentChecked, setConsentChecked] = useState(false);
        const [manufacturerSelectedImage, setManufacturerSelectedImage] = useState(null);
        const [brandSelectedImage, setBrandSelectedImage] = useState(null);
        const [programSelectedImage, setProgramSelectedImage] = useState(null);
        const [drugId, setDrugId] = useState('');
        const [fieldVal, setFieldVal] = useState([]);

        useEffect(() => {
            // Initialize tooltips
            const tooltipNodes = containerRef.current.querySelectorAll('[data-toggle="tooltip"]');
            tooltipNodes.forEach((node) => {
              const tooltip = new window.bootstrap.Tooltip(node);
            });
          }, []);

        const handleCollapse1 = () => {
            setOpenCollapse1(!openCollapse1);
        };

    const handleCollapse2 = () => {
        setOpenCollapse2(!openCollapse2);
    };


    const handleCollapse3 = () => {
        setOpenCollapse3(!openCollapse3);
    };

    const handleCollapse4 = () => {
        setOpenCollapse4(!openCollapse4);
    };

    const handleCollapse5 = () => {
        setOpenCollapse5(!openCollapse5);
    };

    const handleCollapse6 = () => {
        setOpenCollapse6(!openCollapse6);
    };

    const handleManufacturerToggle = () => {
        setManufacturerChecked(!manufacturerChecked);
    };

    const handleBrandToggle = () => {
        setBrandChecked(!brandChecked);
    };

    const handleConsentToggle = () => {
        setConsentChecked(!consentChecked);
    };

    const handleProgramToggle = () => {
        setProgramChecked(!programChecked);
    };

    const handleManufacturerImageUpload = (event) => {
        if (!event.target.files) return;
        const file = event.target.files[0];
        console.log(file);
        if (file) {
            setManufacturerSelectedImage(file);
            if (isOpen) {
            }
        }
    };

    const handleBrandImageUpload = (event) => {
        const file = event.target.files[0];
        if (file) {
            setBrandSelectedImage(file);
            if (isOpen) {
            }
        }
    };

    const handleProgramImageUpload = (event) => {
        const file = event.target.files[0];
        if (file) {
            setProgramSelectedImage(file);
            if (isOpen) {
            }
        }
    };


    const handleManufacturerRemoveImage = () => {
        setManufacturerSelectedImage(null);
    };

    const handleBrandRemoveImage = () => {
        setBrandSelectedImage(null);
    };

    const handleProgramRemoveImage = () => {
        setProgramSelectedImage(null);
    };


    useEffect(() => {
        setIsFormEmpty(formName === '' && drugName === '' && pharmaid === '');
    }, [formName], [drugName], [pharmaid], [formFields], [fieldVal]);

    const addField = (fieldName, fieldId, fieldType, seq, placeholder, isFullDiv) => {
        const field = {
            name: fieldName,
            type: fieldType,
            id: fieldId,
            seq: seq,
            placeholder: placeholder,
            isFullDiv: isFullDiv
        };
        setFormFields([...formFields, field]);
        console.log(formFields);

    };

    const removeSelectedField = (fieldToRemove) => {
        const updatedFields = formFields.filter((field) => field.id !== fieldToRemove);
        setFormFields(updatedFields);
    };


    const handleOpen = () => {
        setIsOpen(true);
    };

    const handleClose = () => {
        setIsOpen(false);
    };

    const handleSubmit = (e) => {
        e.preventDefault();

        const drugNameInput = document.getElementById('drugName');
        const indicationInput = document.getElementById('indication');
        const pageTitleInput = document.getElementById('pageTitle');
        const instructionsInput = document.getElementById('instructions');

        const formData = new FormData();
        formData.append("drugName", drugNameInput.value);
        formData.append("indication", indicationInput.value);
        formData.append("pageTitle", pageTitleInput.value);
        formData.append("instructions", instructionsInput.value);
        formData.append("drugId", 0);

        console.log(manufacturerSelectedImage);
        formData.append("image", new Blob([manufacturerSelectedImage], { type: 'image/jpeg' }), 'manufacturer_logo');
        formData.append("image", new Blob([brandSelectedImage], { type: 'image/jpeg' }), 'brand_logo');
        formData.append("image", new Blob([programSelectedImage], { type: 'image/jpeg' }), 'program_logo');

        const headers = {
            'Content-Type': 'multipart/form-data',
            "Authorization": `${getAuth()}`,
            "UserId": `${getUser()}`

        }
        axios.post(`${getBaseUrl()}/save/drug/brand`, formData, { headers: headers }).
            then(response => {
                if (response.status === 200) {
                    setDrugId(response.data.drugId)
                    alert("Brand details saved successfully");
                }
            });
        handleClose();
    };
      

    const addFormField = (fieldName, fieldId, fieldType, isFullDiv, placeholder, radioValues, seq) => {
        let div = document.createElement('div');
        containerRef.current.appendChild(div);
        div.style.width = "571.25px";
        div.className = isFullDiv ? 'formFieldDiv' : 'formFieldDiv col-md-6';
        div.id = fieldId + "_" + seq
        let label = document.createElement('label');
        label.innerHTML = fieldName;
        label.className = 'col-md-8';
        label.id = fieldId + "_" + seq + "-mainlabel";

        div.appendChild(label);

        // Create and append the trash and pen icons using FontAwesomeIcon
        let iconsContainer = document.createElement('label');
        iconsContainer.className = 'fieldIcons col-md-4 ps';

        const delIcon = fieldId + 'Delete';
        ReactDOM.render(
            <>
                <FontAwesomeIcon icon={faTrashAlt} id={delIcon} className='handPointer' />
                <FontAwesomeIcon icon={faPen} id={fieldId + 'Edit'} className='handPointer' />
            </>,
            iconsContainer
        );


        const handleLabelChange = () => {
            const newLabel = inputField2.value;
            label.innerHTML = newLabel;
        };

        div.appendChild(iconsContainer);

        let bre = document.createElement('br');
        div.appendChild(bre);
        addField(fieldName, fieldId, fieldType, seq, placeholder, isFullDiv);
        console.log(formFields)
        let input = null;

        if (fieldType === 'text') {
            input = document.createElement('input');
            input.className = 'inputField';
            input.style.width = "555.25px"
            input.type = 'text';
            input.placeholder = placeholder ? placeholder : '';

        } else if (fieldType === 'date') {
            input = document.createElement('input');
            input.className = 'col-md-12';
            input.type = 'date';
            input.className = 'inputField';

        } else if (fieldType === 'email') {
            input = document.createElement('input');
            input.placeholder = 'johnsmith@gmail.com';
            input.className = 'col-md-12 inputField';
            input.type = 'email';
            input.name = 'email';

        } else if (fieldType === 'tel') {
            input = document.createElement('input');
            input.placeholder = '(000) 000-0000';
            input.className = 'col-md-12 inputField';
            input.type = 'tel';

        } else if (fieldType === 'radio') {
            for (let i = 0; i < radioValues.length; i++) {
                if (radioValues[i] === 'Other') {
                    let OtherLabel = document.createElement('label');
                    OtherLabel.innerHTML = '&nbspOther';
                    OtherLabel.className = 'ms-4';
                    div.appendChild(OtherLabel);

                    input = document.createElement('input');
                    input.type = 'text';
                    input.name = fieldId;
                    input.value = '';
                    input.className = 'ms-2';

                } else {
                    input = document.createElement('input');
                    input.type = 'radio';
                    input.name = fieldId;
                    input.value = radioValues[i];
                    input.className = i > 0 ? 'radioField ms-4' : 'radioField';

                    let label = document.createElement('label');
                    label.innerHTML = '&nbsp' + radioValues[i];
                    label.className = 'capitalize';
                    div.appendChild(label);
                }
            }
        }

        // Add event listener to the checkbox
        const checkbox = document.createElement('input');
        checkbox.type = 'checkbox';
        checkbox.id = fieldId + 'Switch';
        checkbox.className = `${FormModule.custom_control_input}`;
        checkbox.addEventListener('change', () => {
            input.required = checkbox.checked; // Update the "required" attribute of the input field
        });

        const labelBackground = document.createElement('label');
        labelBackground.className = `${FormModule.custom_control_label}`;
        labelBackground.htmlFor = fieldId + 'Switch';
        labelBackground.style.marginRight = '15px';

        div.appendChild(input);
        div.appendChild(checkbox);
        div.appendChild(labelBackground);

        const showIcons = () => {
            deleteIcon.style.display = 'inline'; // Show the trash icon
            editIcon.style.display = 'inline'; // Show the pen icon
        };

        const hideIcons = () => {
            deleteIcon.style.display = 'none'; // Hide the trash icon
            editIcon.style.display = 'none'; // Hide the pen icon
        };

        const deleteIcon = document.getElementById(delIcon);
        const editIcon = document.getElementById(fieldId + 'Edit');
        const fieldSection = document.getElementById(fieldId + 'Section');
        fieldSection.className = 'disableIpField';

        div.addEventListener('mouseenter', showIcons);
        div.addEventListener('mouseleave', hideIcons);

        deleteIcon.addEventListener('click', () => {
            containerRef.current.removeChild(div);
            containerRef.current.removeChild(bre);
            removeSelectedField(fieldId);
            fieldSection.className = FormModule.inputField;
        });

        deleteIcon.style.display = 'none';
        editIcon.style.display = 'none';
        editIcon.style.marginLeft = '12px';

        editIcon.addEventListener('click', () => {
            containerRef.current.style.width = '49%';
            var container = document.getElementById(fieldId + "-" + seq + "-fieldSetting");
            if (container) {
                container.className = `newContainer col-md-35 ${FormModule.displayBlock} ${FormModule.editContainer} `;
            } else {

                // Create a new container on the right side
                const newContainer = document.createElement('div');
                newContainer.className = `newContainer col-md-35 ${FormModule.displayBlock} ${FormModule.editContainer} `;
                newContainer.style.width = '23.5%';
                newContainer.style.height = '500px';
                newContainer.id = fieldId + "-" + seq + "-fieldSetting";

                // Add Field settings heading to the new container
                const newContainer1 = document.createElement('p');
                newContainer1.innerHTML = 'Field settings';
                newContainer1.className = `newContainer-newContainer1 ${FormModule.fieldSettings}`;
                newContainer.appendChild(newContainer1);

                // Input1
                const input1 = document.createElement('div');
                input1.className = `${FormModule.custom_switch}`;
                const label1 = document.createElement('label');
                label1.htmlFor = fieldId + "-" + seq + "-isRequired";
                label1.className = `${FormModule.toggle_label}`;
                label1.innerHTML = 'Required field';
                // Apply the specified CSS to the label
                label1.style.fontFamily = 'Fira Sans';
                label1.style.fontStyle = 'normal';
                label1.style.fontWeight = '400';
                label1.style.fontSize = '16px';
                label1.style.lineHeight = '28px';
                label1.style.color = '#2D2D2D';
                label1.style.flex = 'none';
                label1.style.paddingLeft = '10px';
                input1.appendChild(label1);
                input1.style.paddingTop = '50px';
                const checkbox1 = document.createElement('input');
                checkbox1.type = 'checkbox';
                checkbox1.className = `${FormModule.custom_control_input}`;
                checkbox1.id = fieldId + "-" + seq + "-isRequired";
                // Add necessary event listeners and checked state
                input1.appendChild(checkbox1);
                const labelBackground1 = document.createElement('label');
                labelBackground1.className = `${FormModule.custom_control_label}`;
                labelBackground1.htmlFor = fieldId + "-" + seq + "-isRequired";
                labelBackground1.style.marginRight = '15px'
                input1.appendChild(labelBackground1);
                newContainer.appendChild(input1);


                // Input2
                const input2 = document.createElement('div');
                const inputContainer2 = document.createElement('div');
                inputContainer2.style.padding = '10px';
                inputContainer2.style.paddingTop = '35px';
                inputContainer2.className = `${FormModule.option_label}`;

                const label2 = document.createElement('label');
                label2.htmlFor = fieldId + "-" + seq + "-label";
                label2.className = `${FormModule.toggle_label}`;
                label2.innerHTML = 'Label';
                inputContainer2.appendChild(label2);

                const inputField2 = document.createElement('input');
                inputField2.type = 'text';
                inputField2.id = fieldId + "-" + seq + "-label";
                inputField2.className = `${FormModule.fieldSettingInput}`;
                inputContainer2.appendChild(document.createElement('br'));
                inputContainer2.appendChild(inputField2);
                input2.appendChild(inputContainer2);
                newContainer.appendChild(input2);

                const saveButton1 = document.getElementById('saveButton');
                if (saveButton1) {
                    saveButton1.addEventListener('click', handleSave);
                } else {
                    console.error("Save button not found.");
                }

                // Input3
                const input3 = document.createElement('div');
                const inputContainer3 = document.createElement('div'); // Create a new div for input3
                inputContainer3.style.padding = '10px'; // Apply padding of 10px
                inputContainer3.style.paddingTop = '20px';
                inputContainer3.className = `${FormModule.option_label}`;
                const label3 = document.createElement('label');
                label3.htmlFor = fieldId + "-" + seq + "-helptext";
                label3.className = `${FormModule.toggle_label} col-md-10`;
                label3.innerHTML = 'Helper text';
                inputContainer3.appendChild(label3);

                const optionalText = document.createElement('span');
                optionalText.innerHTML = 'Optional';
                optionalText.style.fontStyle = 'italic';
                optionalText.style.fontWeight = '300';
                optionalText.style.color = '#797977';
                optionalText.style.fontSize = '14px';// Add left margin to create space between label and optional text
                inputContainer3.appendChild(optionalText);

                const inputField3 = document.createElement('input');
                inputField3.type = 'text';
                inputField3.id = fieldId + "-" + seq + "-helptext";
                inputField3.className = `${FormModule.fieldSettingInput}`;
                inputContainer3.appendChild(document.createElement('br'));
                inputContainer3.appendChild(inputField3);
                input3.appendChild(inputContainer3); // Append inputContainer3 instead of input3
                newContainer.appendChild(input3);


                const saveButtonDiv = document.createElement('div');
                saveButtonDiv.style.padding = '70px';
                saveButtonDiv.style.display = 'flex';
                saveButtonDiv.style.justifyContent = 'center';
                const saveButton = document.createElement('button');
                saveButton.innerHTML = 'Save';
                saveButton.style.background = '#FFFFFF';
                saveButton.style.borderColor = '#382E4C';
                saveButton.style.color = '#382E4C';
                saveButton.style.padding = '12px 24px';
                saveButton.style.cursor = 'pointer';
                saveButton.style.fontSize = '16px';
                saveButton.style.fontWeight = '500';
                saveButton.style.lineHeight = '24px';
                saveButton.style.textTransform = 'uppercase';
                saveButton.style.borderRadius = '4px';
                saveButton.addEventListener("click", function () {
                    containerRef.current.style.width = '';
                    newContainer.className = `newContainer col-md-35 ${FormModule.displayNone} ${FormModule.editContainer}`
                    document.getElementById(fieldId + "_" + seq + "-mainlabel").textContent = document.getElementById(fieldId + "-" + seq + "-label").value;
                });
                saveButtonDiv.appendChild(saveButton);
                newContainer.appendChild(saveButtonDiv);



                // Add closing option to the new container
                const closeIcon = document.createElement('div');
                closeIcon.className = 'closeIcon';
                closeIcon.style.position = 'absolute';
                closeIcon.style.top = '0';
                closeIcon.style.right = '0';
                closeIcon.style.width = '50px';
                closeIcon.style.height = '50px';
                closeIcon.style.display = 'flex';
                closeIcon.style.justifyContent = 'center';
                closeIcon.style.alignItems = 'center';
                closeIcon.style.cursor = 'pointer';
                closeIcon.style.fontSize = '20px';
                closeIcon.style.color = '#FFFFFF';
                closeIcon.addEventListener('click', () => {
                    containerRef.current.style.width = ''; // Reset the width of the main container
                    // containerRef.current.parentElement.removeChild(newContainer);
                    newContainer.className = `newContainer col-md-35 ${FormModule.displayNone} ${FormModule.editContainer}`
                });

                const closeIconContent = document.createElement('span');
                ReactDOM.render(<FontAwesomeIcon icon={faX} />, closeIconContent);
                closeIcon.appendChild(closeIconContent);

                newContainer.appendChild(closeIcon);

                // Append the new container to the parent container
                containerRef.current.parentElement.appendChild(newContainer);

            }


        });

        containerRef.current.appendChild(bre);
    }

    ////////////////
    // const SignatureField = () => {
    // const [canvas, setCanvas] = useState(null);
    // const [signatureData, setSignatureData] = useState(null);

    // const handleCanvasRef = (ref) => {
    //     if (ref) {
    //         setCanvas(ref);
    //         const canvasContext = ref.getContext("2d");
    //         // Configure canvas properties (e.g., stroke style, line width, etc.)
    //         // ...
    //     }
    // };

    // const handleClearCanvas = () => {
    //     if (canvas) {
    //         const canvasContext = canvas.getContext("2d");
    //         canvasContext.clearRect(0, 0, canvas.width, canvas.height);
    //         setSignatureData(null);
    //     }
    // };

    // const handleSaveSignature = () => {
    //     if (canvas) {
    //         const dataUrl = canvas.toDataURL();
    //         setSignatureData(dataUrl);
    //     }
    // };
    // }


    /////////////////////

    //Patient information
    const Firstname = () => {
        addFormField('First name', 'firstName', 'text', true, '', '', 1);
    };

    const Lastname = () => {
        addFormField('Last name', 'lastName', 'text', true, '', '', 2);
    };

    const Dob = () => {
        addFormField('Date of birth', 'dob', 'date', false, '', '', 3);
    };

    const Sex = () => {
        const argument = ['male', 'female'];
        const radioGroup = document.createElement('div');
        const label = document.createElement('label');
        label.textContent = 'Sex';

        argument.forEach((option) => {
            const input = document.createElement('input');
            input.type = 'radio';
            input.name = 'sex';
            input.value = option;
            radioGroup.appendChild(input);
            const optionLabel = document.createElement('span');
            optionLabel.textContent = option;
            radioGroup.appendChild(optionLabel);
        });

        addFormField('Sex', 'sex', 'radio', false, '', '', 4, radioGroup);

        return null;
    };


    const Address1 = () => {
        addFormField('Address line 1', 'address1', 'text', false, '', '', 5);
    };

    const Address2 = () => {
        addFormField('Address line 2', 'address2', 'text', false, '', '', 6);
    };

    const City = () => {
        addFormField('City', 'city', 'text', true, '', '', 7);
    };

    const State = () => {
        addFormField('State', 'state', 'text', true, '', '', 8);
    };

    const Zip_code = () => {
        addFormField('Zip code', 'zipCode', 'text', true, '', '', 9);
    };

    const Email = () => {
        addFormField('Email', 'email', 'email', true, '', '', 10);
    };


    const Phone = () => {
        addFormField('Cell phone', 'cellPhone', 'tel', true, '', '', 11);
    };


    const Contact = () => {
        const argument = ['Phone', 'Text', 'Email'];
        addFormField('Preferred contact method', 'contact', 'radio', true, '', argument, 12);
    };

    const Language = () => {
        const argument = ['English', 'Spanish', 'Other'];
        addFormField('Preferred language', 'language', 'radio', true, '', argument, 13);
    };

    const SSN = () => {
        addFormField('Last 4 digits of SSN', 'ssn', 'text', true, ' 0000', '', 14);
    };

    const FullName = () => {
        addFormField('Full name', 'fullname', 'text', true, '', '', 15);
    };

    const PatientRelation = () => {
        const values = ['Self', 'Other'];
        addFormField('Relationship to patient', 'patientRelation', 'radio', true, '', values, 17);
    };

    const FormCopy = () => {
        addFormField('Form copy', 'formCopy', 'text', true, '', '', 18);
    };

    const Signature = () => {
        addFormField('Signature', 'signature', 'text', true, '', '', 19);
    };

    const OfficeAddress1 = () => {
        addFormField('Address line 1', 'officeAddress1', 'text', false, '', '', 5);
    };

    const OfficeAddress2 = () => {
        addFormField('Address line 2', 'officeAddress2', 'text', false, '', '', 6);
    };

    const OfficeCity = () => {
        addFormField('City', 'officeCity', 'text', true, '', '', 7);
    };

    const OfficeState = () => {
        addFormField('State', 'officeState', 'text', true, '', '', 8);
    };

    const OfficeZip_code = () => {
        addFormField('Zip code', 'officeZipCode', 'text', true, '', '', 9);
    };


    const OfficePhone = () => {
        addFormField('Office Phone', 'officePhone', 'tel', true, '', '', 20);
    };

    const Officefax = () => {
        addFormField('Fax', 'officeFax', 'tel', true, '', '', 21);
    };

    const doesInsurance = () => {
        const values = ['Self', 'Other'];
        addFormField('Does patient have insurance?', 'doesInsurance', 'radio', true, '', values, 22);
    };

    const insuranceCompanyName = () => {
        addFormField('Insurance company name', 'insuranceCompanyName', 'text', true, '', '', 23);
    };

    const insurancePolicyHolder = () => {
        addFormField('Insurance policy holder', 'insurancePolicyHolder', 'text', true, '', '', 24);
    };

    const policyHolderRelationship = () => {
        addFormField('Policy holder relationship to patient', 'policyHolderRelationship', 'text', true, '', '', 25);
    };

    const insuranceNumberOrMemberID = () => {
        addFormField('Insurance number or member ID', 'insuranceNumberOrMemberID', 'text', true, '', '', 26);
    };

    const insuranceGroupNumber = () => {
        addFormField('Insurance group number', 'insuranceGroupNumber', 'text', true, '', '', 27);
    };

    const prescriptionInsuranceCompany = () => {
        addFormField('Prescription insurance company name', 'prescriptionInsuranceCompany', 'text', true, '', '', 28);
    };

    const prescriptionInsurancePolicyHolder = () => {
        addFormField('Prescription insurance policy holder', 'prescriptionInsurancePolicyHolder', 'text', true, '', '', 29);
    };

    const prescriptionInsuranceBIN = () => {
        addFormField('Prescription insurance BIN', 'prescriptionInsuranceBIN', 'text', true, '', '', 30);
    };

    const prescriptionInsurancePCN = () => {
        addFormField('Prescription insurance PCN', 'prescriptionInsurancePCN', 'text', true, '', '', 31);
    };

    const prescriptionInsuranceNumberOrMemberID = () => {
        addFormField('Prescription insurance number or member ID', 'prescriptionInsuranceNumberOrMemberID', 'text', true, '', '', 32);
    };

    const prescriptionInsuranceGroupNumber = () => {
        addFormField('Prescription insurance group number', 'prescriptionInsuranceGroupNumber', 'text', true, '', '', 33);
    };

    const doesPatientHaveSecondaryInsurance = () => {
        addFormField('Does patient have secondary insurance?', 'doesPatientHaveSecondaryInsurance', 'radio', true, '', '', 34);
    };

    const PrimaryDiagnosis = () => {
        addFormField('Primary diagnosis', 'PrimaryDiagnosis', 'radio', true, '', '', 35);
    };

    const Allergies = () => {
        addFormField('Allergies', 'Allergies', 'text', true, '', '', 36);
    };

    const PreviouslyTriedMedication = () => {
        addFormField('Previously tried medication', 'PreviouslyTriedMedication', 'text', true, '', '', 37);
    };

    const PatientWeight = () => {
        addFormField('Patient weight', 'PatientWeight', 'text', true, '', '', 38);
    };

    const PatientHeight = () => {
        addFormField('Patient height', 'PatientHeight', 'text', true, '', '', 39);
    };

    const SupportingDocumentation = () => {
        addFormField('Supporting documentation', 'SupportingDocumentation', 'text', true, '', '', 40);
    };
    //
    const Script = () => {
        addFormField('Script', 'Script', 'text', true, '', '', 41);
    };

    const Pharmacy = () => {
        addFormField('Pharmacy', 'Pharmacy', 'text', true, 42);
    };

    const DispenseAsWritten = () => {
        addFormField('Dispense as written', 'DispenseAsWritten', 'text', true, 43);
    };

    const DeliveryInformation = () => {
        addFormField('Delivery information', 'DeliveryInformation', 'text', true, '', '', 44);
    };

    function saveFieldValues(fieldId, seq) {
        const isRequired = document.getElementById(`${fieldId}-${seq}-isRequired`).checked;
        const label = document.getElementById(`${fieldId}-${seq}-label`).value;
        const helptext = document.getElementById(`${fieldId}-${seq}-helptext`).value;
        console.log(fieldVal)
        const fieldObj = [{
            isRequired: isRequired,
            label: label,
            helptext: helptext,
            seq: seq
        }]
        // setFieldVal([...fieldVal,fieldObj]);
        setFieldVal(preFieldVal => ([...preFieldVal, ...fieldObj]));

        console.log("formFields __", fieldVal);

    }

    const saveDynamicForm = async () => {
        console.log(formFields);
        const selectedFieldsArray = formFields.map((field) => ({
            name: field.name,
            type: field.type,
            id: field.id,
            seq: field.seq,
            placeholder: field.placeholder,
            isFullDiv: field.isFullDiv,
            isRequired: document.getElementById(field.id + "-" + field.seq + "-isRequired") ? document.getElementById(field.id + "-" + field.seq + "-isRequired").checked : false,
            label: document.getElementById(field.id + "-" + field.seq + "-label") ? document.getElementById(field.id + "-" + field.seq + "-label").checked : false,
            helptext: document.getElementById(field.id + "-" + field.seq + "-helptext") ? document.getElementById(field.id + "-" + field.seq + "-helptext").checked : false,
        }));
        const selectedFieldsJson = { fields: selectedFieldsArray };
        selectedFieldsJson.fields.sort((a, b) => a.seq - b.seq);
        var data = { formTemplateDef: selectedFieldsJson, drugId: drugId, formTemplateId: 0 }
        console.log(data)
        const headers = getHeaders()
        const response = await fetchAPI('/save/form/template', {
            method: 'POST',
            headers: headers,
            body: JSON.stringify(data)
        });
        let result = await response.JSON();
        if (result.status == 200) {
            alert("Form Template Saved Successfully");
            router.push("../dashboards/admin");
        } else {
            alert("Not Saved");
        }
    }




    return (
        <div className={` ${LoginModule.bodyFont}`}>
            <div className={` ${FormModule.borderForm} row`}>
                <div className={` ${FormModule.centerSelf} col-md-3`}>
                    <span className='ms-2 pe-2 ps-4'><FontAwesomeIcon icon={faChevronLeft} /></span>
                        <Link href={'../dashboards/admin'} className={`${FormModule.backToAllForms}`}>Back to all forms</Link>
                </div>
                <div className='col-md-9'>
                    <div className={` ${FormModule.previewContainer} ${FormModule.centered_div} me-4`}>
                        <p className={`col-md-1 ${FormModule.preview} pb-2 pt-4 pe-5`}>Preview</p>
                        <button className={` ${FormModule.publishButton} mt-1`} onClick={saveDynamicForm}>Publish</button>
                    </div>
                </div>

            </div>
            <div className='row '>
                <div className={`col-md-3 ps-0`}>
                    <div className={`${FormModule.navbarForm}  ${FormModule.width100p} ps-5 pt-5`}>
                        <name className={`${FormModule.availField} `}> Available fields</name>
                        <p className={`${FormModule.formSetting} handPointer pt-4`} onClick={handleOpen}>Brand details</p>
                        {isOpen && (
                            <div className="popup">
                                <div className={`${FormModule.formPopup} `}>
                                    <h2 className={`${FormModule.brandName} `}>Brand Details</h2>
                                    <div className={FormModule.closeIcon} onClick={handleClose}>
                                        <Image src={Close} alt="Close Icon" className={`${FormModule.closeIcon} `} width={16.5} height={16.5} />
                                    </div>
                                    <form onSubmit={handleSubmit} className={`${FormModule.FormCss} mt-4`}>

                                        <div>
                                            <label htmlFor="drugName">Drug Name</label>
                                            <input type="text" id="drugName" className={`${FormModule.popInput} mb-3`} placeholder="Enter drug name" value={drugName} onChange={(e) => setDrugname(e.target.value)} />
                                        </div>
                                        <div>
                                            <label htmlFor="indication">Indication</label>
                                            <input type="text" id="indication" className={`${FormModule.popInput} mb-3`} placeholder="Enter indication" value={indication} onChange={(e) => setIndication(e.target.value)} />
                                        </div>
                                        <div >
                                            <label htmlFor="pageTitle">Page Title</label>
                                            <input type="text" id="pageTitle" className={`${FormModule.popInput} mb-3`} placeholder="Enter page title" value={pageTitle} onChange={(e) => setPageTitle(e.target.value)} />
                                        </div>
                                        <div>
                                            <label htmlFor="instructions">Instructions</label>
                                            <input id="instructions" className={`${FormModule.popInput} mb-3`} placeholder="Enter instructions" value={instructions} onChange={(e) => setInstructions(e.target.value)} />
                                        </div>


                                        <div>
                                            <div className={`${FormModule.custom_switch}`}>
                                                <label htmlFor="manufacturerSwitch" className={`${FormModule.toggle_label}`}>
                                                    Manufacturer Logo
                                                </label>
                                                <input type="checkbox" className={`${FormModule.custom_control_input} `} id="manufacturerSwitch"
                                                    checked={manufacturerChecked} onChange={handleManufacturerToggle} />
                                                <label className={`${FormModule.custom_control_label}`} htmlFor="manufacturerSwitch"></label>
                                            </div>

                                            {manufacturerChecked && (
                                                <div className={`${FormModule.form_group_container} ${manufacturerChecked ? 'active' : ''}`}>
                                                    <div className={`${FormModule.row}`}>
                                                        <div className={`${FormModule.column}`}>
                                                            <input type="file" accept="image/*" id="manufacturerUploadInput" className="sr-only" onChange={handleManufacturerImageUpload} />
                                                            <div className={`${FormModule.preview}`}>
                                                                <div className={`${FormModule.imageBox}`}>
                                                                    {manufacturerSelectedImage ? (
                                                                        <img src={URL.createObjectURL(manufacturerSelectedImage)} className={`${FormModule.uploadedImage}`} alt="Preview" width={181} height={101} />
                                                                    ) : (
                                                                        <div className={`${FormModule.emptyBox}`}>Image not yet selected</div>
                                                                    )}
                                                                </div>
                                                            </div>
                                                        </div>
                                                        <div className={`${FormModule.column}`}>
                                                            <label htmlFor="manufacturerUploadInput" className={`${FormModule.buttonUpload}`}>
                                                                UPLOAD
                                                            </label>
                                                            <br />
                                                            <label className={`${FormModule.removeButton}`} onClick={handleManufacturerRemoveImage}>
                                                                REMOVE
                                                            </label>
                                                        </div>
                                                    </div>

                                                </div>
                                            )}
                                        </div>


                                        <div className={`${FormModule}`}>
                                            <div className={`${FormModule.custom_switch}`}>
                                                <label htmlFor="brandSwitch" className={`${FormModule.toggle_label}`}>
                                                    Brand Logo
                                                </label>
                                                <input type="checkbox" className={`${FormModule.custom_control_input}`}
                                                    id="brandSwitch" checked={brandChecked} onChange={handleBrandToggle} />
                                                <label className={`${FormModule.custom_control_label}`} htmlFor="brandSwitch"></label>
                                            </div>

                                            {brandChecked && (
                                                <div className={`${FormModule.form_group_container} ${brandChecked ? 'active' : ''}`}>
                                                    <div className={`${FormModule.row}`}>
                                                        <div className={`${FormModule.column}`}>
                                                            <input type="file" accept="image/*" id="brandUploadInput" className="sr-only" onChange={handleBrandImageUpload} />
                                                            <div className={`${FormModule.preview}`}>
                                                                <div className={`${FormModule.imageBox}`}>
                                                                    {brandSelectedImage ? (
                                                                        <img src={URL.createObjectURL(brandSelectedImage)} alt="Preview" className={`${FormModule.uploadedImage}`} width={181} height={101} />
                                                                    ) : (
                                                                        <div className={`${FormModule.emptyBox}`}>Image not yet selected</div>
                                                                    )}
                                                                </div>
                                                            </div>
                                                        </div>
                                                        <div className={`${FormModule.column}`}>
                                                            <label htmlFor="brandUploadInput" className={`${FormModule.buttonUpload}`}>
                                                                UPLOAD
                                                            </label>
                                                            <br />
                                                            <label className={`${FormModule.removeButton}`} onClick={handleBrandRemoveImage}>
                                                                REMOVE
                                                            </label>
                                                        </div>
                                                    </div>

                                                </div>
                                            )}
                                        </div>

                                        <div >
                                            <div className={`${FormModule.custom_switch}`}>
                                                <label htmlFor="programSwitch" className={`${FormModule.toggle_label}`}>
                                                    Program Logo
                                                </label>
                                                <input type="checkbox" className={`${FormModule.custom_control_input}`}
                                                    id="programSwitch" checked={programChecked} onChange={handleProgramToggle} />
                                                <label className={`${FormModule.custom_control_label}`} htmlFor="programSwitch"></label>
                                            </div>

                                            {programChecked && (
                                                <div className={`${FormModule.form_group_container} ${programChecked ? 'active' : ''}`}>
                                                    <div className={`${FormModule.row}`}>
                                                        <div className={`${FormModule.column}`}>
                                                            <input type="file" accept="image/*" id="programUploadInput" className="sr-only" onChange={handleProgramImageUpload} />
                                                            <div className={`${FormModule.preview}`}>
                                                                <div className={`${FormModule.imageBox}`}>
                                                                    {programSelectedImage ? (
                                                                        <img src={URL.createObjectURL(programSelectedImage)} alt="Preview" className={`${FormModule.uploadedImage}`} width={60} height={60} />
                                                                    ) : (
                                                                        <div className={`${FormModule.emptyBox}`}>Image not yet selected</div>
                                                                    )}
                                                                </div>
                                                            </div>
                                                        </div>
                                                        <div className={`${FormModule.column}`}>
                                                            <label htmlFor="programUploadInput" className={`${FormModule.buttonUpload}`}>
                                                                UPLOAD
                                                            </label>
                                                            <br />
                                                            <label className={`${FormModule.removeButton}`} onClick={handleProgramRemoveImage}>
                                                                REMOVE
                                                            </label>
                                                        </div>
                                                    </div>

                                                </div>
                                            )}
                                        </div>


                                        <button className={`${FormModule.saveButton} mb-3`} type="submit">SAVE</button>
                                    </form>
                                </div>
                            </div>
                        )}
                    </div>
                    <div className={`${FormModule.sections} ps-2 `}>

                        <div className={`align-items-center d-flex p-2 handPointer`} style={{ backgroundColor: open ? '#F3F1F5' : 'initial', borderBottom: open ? '1.5px solid #8A7BA7' : 'initial', boxShadow: open ? '0px 1px 2px rgba(0, 0, 0, 0.15)' : 'initial' }}
                            onClick={handleCollapse1} aria-controls="collapse1" aria-expanded={openCollapse1}>
                            {openCollapse1 && (
                                <FontAwesomeIcon className='grey' icon={faChevronDown} />
                            )}

                            {!openCollapse1 && (
                                <FontAwesomeIcon className='grey' icon={faChevronRight} />
                            )}

                            <span className={`ms-3 grey `}>Patient information</span>
                        </div>

                        <Collapse in={openCollapse1}>
                            <div id="collapse1">
                                <ul id="frmb-0-control-box" className={` ${FormModule.formcontrol} mt-3 ms-3`}>

                                    <li className={`${FormModule.inputField}`} onClick={Firstname} id='firstNameSection' data-toggle="tooltip" title="Tooltip for First name">
                                        <span className={`${FormModule.abc} `}>abc</span>
                                        <p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`} >First name</p>
                                    </li>
                                    <li draggable="true" className={`${FormModule.inputField}`} label="Lastname" onClick={Lastname} id='lastNameSection'>
                                        <span className={`${FormModule.abc} `}>abc</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`} >Last name</p></li>

                                    <li label="Dob" draggable="true" className={`${FormModule.inputField}`} onClick={Dob} id='dobSection'>
                                        <div style={{ marginLeft: '20px', display: 'inline-block' }}>
                                            <Image src={Calendar} alt="Calendar" width={15} height={15} />
                                        </div>
                                        <p className={`${FormModule.inputField} mb-1 ms-1 ps-4 pt-2 w-auto overflow-visible`} style={{ display: 'inline-block', margin: '0' }}>
                                            Date of birth</p></li>

                                    <li label="Sex" draggable="true" className={`${FormModule.inputField}`} onClick={Sex} id='sexSection'>
                                        <div className={`${FormModule.circle_container} `}>
                                            <div className={`${FormModule.outer_circle}`}>
                                                <div className={`${FormModule.inner_circle}`}>
                                                    <div className={`${FormModule.red_circle}`}></div>
                                                </div>
                                            </div>
                                            <div className={`${FormModule.outer_circle1}`}>
                                                <div className={`${FormModule.inner_circle1}`}>
                                                    <div className={`${FormModule.red_circle1}`}></div>
                                                </div>
                                            </div>
                                            <div className='ms-3'>
                                                Sex
                                            </div>
                                        </div>

                                    </li>

                                    <li label="Address1" draggable="true" data-toggle="tooltip" data-placement="top" title="Tooltip on top"   className={`${FormModule.inputField}`} onClick={Address1} id='address1Section'>
                                        <span className={`${FormModule.abc} `}>abc</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Address line 1</p></li>

                                    <li label="Address2" draggable="true" className={`${FormModule.inputField}`} onClick={Address2} id='address2Section'>
                                        <span className={`${FormModule.abc} `}>abc</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Address line 2</p></li>

                                    <li label="City" draggable="true" className={`${FormModule.inputField}`} onClick={City} id='citySection'>
                                        <span className={`${FormModule.abc} `}>abc</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>City</p></li>

                                    <li label="State" draggable="true" className={`${FormModule.inputField}`} onClick={State} id='stateSection'>
                                        <span className={`${FormModule.abc} `}>abc</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>State</p></li>

                                    <li label="Zip_code" draggable="true" className={`${FormModule.inputField}`} onClick={Zip_code} id='zipCodeSection'>
                                        <span className={`${FormModule.abc} `}>123</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Zip code</p></li>

                                    <li label="Email" draggable="true" className={`${FormModule.inputField}`} onClick={Email} id='emailSection'>
                                        <span className={`${FormModule.abc} `}>abc</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Email</p></li>

                                    <li label="Phone" draggable="true" className={`${FormModule.inputField}`} onClick={Phone} id='cellPhoneSection'>
                                        <span className={`${FormModule.abc} `}>123</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Cell phone</p></li>

                                    <li label="Contact" draggable="true" className={`${FormModule.inputField}`} onClick={Contact} id='contactSection'>
                                        <div className={`${FormModule.circle_container} `}>
                                            <div className={`${FormModule.outer_circle}`}>
                                                <div className={`${FormModule.inner_circle}`}>
                                                    <div className={`${FormModule.red_circle}`}></div>
                                                </div>
                                            </div>
                                            <div className={`${FormModule.outer_circle1}`}>
                                                <div className={`${FormModule.inner_circle1}`}>
                                                    <div className={`${FormModule.red_circle1}`}></div>
                                                </div>
                                            </div>
                                            <div className='ms-3'>
                                                Preferred contact method
                                            </div>
                                        </div>
                                    </li>
                                    <li label="Language" draggable="true" className={`${FormModule.inputField}`} onClick={Language} id='languageSection'>
                                        <span className={`${FormModule.abc} `}>abc</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}> Preferred language </p></li>

                                    <li label="SSN" draggable="true" className={`${FormModule.inputField}`} onClick={SSN} id='ssnSection'>
                                        <span className={`${FormModule.abc} `}>123</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Last 4 digits of SSN</p></li>
                                </ul>

                            </div>
                        </Collapse>



                        {/* ***** section 2  ******* */}

                        <div className={`align-items-center d-flex mt-4 p-2`} style={{ backgroundColor: open ? '#F3F1F5' : 'initial', borderBottom: open ? '1.5px solid #8A7BA7' : 'initial', boxShadow: open ? '0px 1px 2px rgba(0, 0, 0, 0.15)' : 'initial' }}
                            onClick={handleCollapse2} aria-controls="collapse2" aria-expanded={openCollapse2}>
                            {openCollapse2 && (
                                <FontAwesomeIcon className='grey' icon={faChevronDown} />
                            )}

                            {!openCollapse2 && (
                                <FontAwesomeIcon className='grey' icon={faChevronRight} />
                            )}
                            <span className={`ms-3 grey handPointer`}>Patient consent</span>

                        </div>

                        <Collapse in={openCollapse2}>
                            <div id="collapse2">

                                <div className={`${FormModule.custom_switch} mb-5 mt-1`}>
                                    <label className={`fa-1x fw-normal ms-5 mt-4`}>
                                        Enable patient consent
                                    </label>
                                    <input type="checkbox" className={`${FormModule.custom_control_input} `}
                                        id="consentSwitch" checked={consentChecked} onChange={handleConsentToggle} />
                                    <label className={`${FormModule.custom_control_label} mt-4 me-5`} htmlFor="consentSwitch"></label>
                                </div>

                                <ul id="frmb-0-control-box" className={` ${FormModule.formcontrol} mt-3 ms-3`}>
                                    <li draggable="true" className={`${FormModule.inputField} ${consentChecked ? '' : FormModule.disabled}`} label="FullName" onClick={consentChecked ? FullName : undefined} id='nameSection'>
                                        <span className={`${FormModule.abc} `}>abc</span>
                                        <p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Full Name</p>
                                    </li>

                                    <li label="patientRelation" draggable="true" className={`${FormModule.inputField} ${consentChecked ? '' : FormModule.disabled}`}
                                        onClick={consentChecked ? PatientRelation : undefined} id='patientRelationSection' >
                                        <div className={`${FormModule.circle_container} `}>
                                            <div className={`${FormModule.outer_circle}`}>
                                                <div className={`${FormModule.inner_circle}`}>
                                                    <div className={`${FormModule.red_circle}`}></div>
                                                </div>
                                            </div>
                                            <div className={`${FormModule.outer_circle1}`}>
                                                <div className={`${FormModule.inner_circle1}`}>
                                                    <div className={`${FormModule.red_circle1}`}></div>
                                                </div>
                                            </div>
                                            <div className='ms-3'>
                                                Relationship to patient
                                            </div>
                                        </div>
                                    </li>

                                    <li draggable="true" className={`${FormModule.inputField} ${consentChecked ? '' : FormModule.disabled}`}
                                        label="FormCopy" onClick={consentChecked ? FormCopy : undefined} id='formCopySection'>
                                        <span className={`${FormModule.abc} `}>abc</span>
                                        <p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Consent body copy</p>
                                    </li>

                                    <li label="Signature" draggable="true" className={`${FormModule.inputField} ${consentChecked ? '' : FormModule.disabled}`}
                                        onClick={consentChecked ? Signature : undefined} id='signatureSection' ><span className={`${FormModule.abc} `}>abc</span>
                                        <p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Signature code</p>
                                    </li>
                                </ul>
                            </div>
                        </Collapse>

                        {/* ***** section 3  ******* */}

                        <div className={`align-items-center d-flex mt-4 p-2 handPointer`} style={{ backgroundColor: open ? '#F3F1F5' : 'initial', borderBottom: open ? '1.5px solid #8A7BA7' : 'initial', boxShadow: open ? '0px 1px 2px rgba(0, 0, 0, 0.15)' : 'initial' }}
                            onClick={handleCollapse3} aria-controls="collapse2" aria-expanded={openCollapse2}>
                            {openCollapse3 && (
                                <FontAwesomeIcon className='grey' icon={faChevronDown} />
                            )}

                            {!openCollapse3 && (
                                <FontAwesomeIcon className='grey' icon={faChevronRight} />
                            )}
                            <span className={`ms-3 grey`}>Office information</span>
                        </div>

                        <Collapse in={openCollapse3}>
                            <div id="collapse3">
                                <ul id="frmb-0-control-box" className={` ${FormModule.formcontrol} mt-3 ms-3`}>

                                    <li label="OfficeAddress1" draggable="true" className={`${FormModule.inputField}`} onClick={OfficeAddress1} id='officeAddress1Section'>
                                        <span className={`${FormModule.abc} `}>abc</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Address line 1</p></li>

                                    <li label="OfficeAddress2" draggable="true" className={`${FormModule.inputField}`} onClick={OfficeAddress2} id='officeAddress2Section'>
                                        <span className={`${FormModule.abc} `}>abc</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Address line 2</p></li>

                                    <li label="OfficeCity" draggable="true" className={`${FormModule.inputField}`} onClick={OfficeCity} id='officeCitySection'>
                                        <span className={`${FormModule.abc} `}>abc</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>City</p></li>

                                    <li label="OfficeState" draggable="true" className={`${FormModule.inputField}`} onClick={OfficeState} id='officeStateSection'>
                                        <span className={`${FormModule.abc} `}>abc</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>State</p></li>

                                    <li label="OfficeZip_code" draggable="true" className={`${FormModule.inputField}`} onClick={OfficeZip_code} id='officeZipcodeSection'>
                                        <span className={`${FormModule.abc} `}>123</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Zip code</p></li>

                                    <li label="OfficePhone" draggable="true" className={`${FormModule.inputField}`} onClick={OfficePhone} id='officePhoneSection'>
                                        <span className={`${FormModule.abc} `}>123</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Office phone</p></li>

                                    <li label="Officefax" draggable="true" className={`${FormModule.inputField}`} onClick={Officefax} id='officefaxSection'>
                                        <span className={`${FormModule.abc} `}>123</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}> Office fax</p></li>

                                </ul>
                            </div>
                        </Collapse>

                        {/* ***** section 4  ******* */}

                        <div className={`align-items-center d-flex mt-4 p-2 handPointer`} style={{ backgroundColor: open ? '#F3F1F5' : 'initial', borderBottom: open ? '1.5px solid #8A7BA7' : 'initial', boxShadow: open ? '0px 1px 2px rgba(0, 0, 0, 0.15)' : 'initial' }}
                            onClick={handleCollapse4} aria-controls="collapse4" aria-expanded={openCollapse4}>
                            {openCollapse4 && (
                                <FontAwesomeIcon className='grey' icon={faChevronDown} />
                            )}

                            {!openCollapse4 && (
                                <FontAwesomeIcon className='grey' icon={faChevronRight} />
                            )}
                            <span className={`ms-3 grey`}>Insurance information</span>
                        </div>

                        <Collapse in={openCollapse4}>
                            <div id="collapse4">
                                <ul id="frmb-0-control-box" className={` ${FormModule.formcontrol} mt-3 ms-3`}>

                                    <li draggable="true" className={`${FormModule.inputField}`} label="DoesInsurance" onClick={doesInsurance} id='doesInsuranceSection'>
                                        <div style={{ marginLeft: '20px', display: 'inline-block' }}>
                                            <Image src={Checkboxes} alt="checkbox" width={29} height={14} />
                                        </div>
                                        <p className={`${FormModule.inputField} mb-1 ps-4 pt-2 w-auto overflow-visible`} style={{ display: 'inline-block', margin: '0' }}>
                                            Does patient have insurance?
                                        </p>
                                    </li>

                                    <li draggable="true" className={`${FormModule.inputField}`} label="InsuranceCompanyName" onClick={insuranceCompanyName} id='insuranceCompanyNameSection'>
                                        <span className={`${FormModule.abc} `}>abc</span>
                                        <p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Insurance company name</p>
                                    </li>

                                    <li draggable="true" className={`${FormModule.inputField}`} label="InsurancePolicyHolder" onClick={insurancePolicyHolder} id='insurancePolicyHolderSection'>
                                        <span className={`${FormModule.abc} `}>abc</span>
                                        <p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Insurance policy holder</p>
                                    </li>

                                    <li draggable="true" className={`${FormModule.inputField}`} label="PolicyHolderRelationship" onClick={policyHolderRelationship} id='policyHolderRelationshipSection'>
                                        <span className={`${FormModule.abc} `}>abc</span>
                                        <p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Policy holder relationship to pati..</p>
                                    </li>

                                    <li draggable="true" className={`${FormModule.inputField}`} label="InsuranceNumber" onClick={insuranceNumberOrMemberID} id='insuranceNumberOrMemberIDSection'>
                                        <span className={`${FormModule.abc} `}>123</span>
                                        <p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Insurance number or member ID</p>
                                    </li>

                                    <li draggable="true" className={`${FormModule.inputField}`} label="InsuranceGroupNumber" onClick={insuranceGroupNumber} id='insuranceGroupNumberSection'>
                                        <span className={`${FormModule.abc} `}>123</span>
                                        <p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Insurance group number</p>
                                    </li>

                                    <li draggable="true" className={`${FormModule.inputField}`} label="PrescriptionInsuranceCompanyName" onClick={prescriptionInsuranceCompany} id='prescriptionInsuranceCompanySection'>
                                        <span className={`${FormModule.abc} `}>abc</span>
                                        <p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Prescription insurance company name</p>
                                    </li>

                                    <li draggable="true" className={`${FormModule.inputField}`} label="PrescriptionInsurancePolicyHolder" onClick={prescriptionInsurancePolicyHolder} id='prescriptionInsurancePolicyHolderSection'>
                                        <span className={`${FormModule.abc} `}>abc</span>
                                        <p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Prescription insurance policy holder</p>
                                    </li>

                                    <li draggable="true" className={`${FormModule.inputField}`} label="PrescriptionInsuranceBIN" onClick={prescriptionInsuranceBIN} id='prescriptionInsuranceBINSection'>
                                        <span className={`${FormModule.abc} `}>abc</span>
                                        <p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Prescription insurance BIN</p>
                                    </li>

                                    <li draggable="true" className={`${FormModule.inputField}`} label="PrescriptionInsurancePCN" onClick={prescriptionInsurancePCN} id='prescriptionInsurancePCNSection'>
                                        <span className={`${FormModule.abc} `}>abc</span>
                                        <p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Prescription insurance PCN</p>
                                    </li>

                                    <li draggable="true" className={`${FormModule.inputField}`} label="PrescriptionInsuranceNumber" onClick={prescriptionInsuranceNumberOrMemberID} id='prescriptionInsuranceNumberOrMemberIDSection'>
                                        <span className={`${FormModule.abc} `}>123</span>
                                        <p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2 text-sm text-md-base text-lg-lg`}>Prescription insurance number or member ID</p>
                                    </li>

                                    <li draggable="true" className={`${FormModule.inputField}`} label="PrescriptionInsuranceGroupNumber" onClick={prescriptionInsuranceGroupNumber} id='prescriptionInsuranceGroupNumberSection'>
                                        <span className={`${FormModule.abc} `}>123</span>
                                        <p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Prescription insurance group number</p>
                                    </li>

                                    <li draggable="true" className={`${FormModule.inputField}`} label="DoesPatientHaveSecondaryInsurance" onClick={doesPatientHaveSecondaryInsurance} id='doesPatientHaveSecondaryInsuranceSection'>
                                        <span className={`${FormModule.abc} `}>abc</span>
                                        <p className={`${FormModule.inputField}  mb-1 ms-5 ps-4 pt-2`} style ={{ whiteSpace: 'nowrap', overflow: 'hidden', textOverflow: 'ellipsis', maxWidth: '340px' }}>Does patient have secondary insurance?</p>
                                    </li>

                                </ul>
                            </div>
                        </Collapse>

                        {/* ***** section 5  ******* */}

                        <div className={`align-items-center d-flex mt-4 p-2 handPointer`} style={{ backgroundColor: open ? '#F3F1F5' : 'initial', borderBottom: open ? '1.5px solid #8A7BA7' : 'initial', boxShadow: open ? '0px 1px 2px rgba(0, 0, 0, 0.15)' : 'initial' }}
                            onClick={handleCollapse5} aria-controls="collapse5" aria-expanded={openCollapse5}>
                            {openCollapse5 && (
                                <FontAwesomeIcon className='grey' icon={faChevronDown} />
                            )}

                            {!openCollapse5 && (
                                <FontAwesomeIcon className='grey' icon={faChevronRight} />
                            )}
                            <span className={`ms-3 grey`}>Clinical information</span>
                        </div>

                        <Collapse in={openCollapse5}>
                            <div id="collapse5">
                                <ul id="frmb-0-control-box" className={` ${FormModule.formcontrol} mt-3 ms-3`}>

                                    <li label="PrimaryDiagnosis" draggable="true" className={`${FormModule.inputField}`} onClick={PrimaryDiagnosis} id='PrimaryDiagnosisSection'>
                                        <div className={`${FormModule.circle_container} `}>
                                            <div className={`${FormModule.outer_circle}`}>
                                                <div className={`${FormModule.inner_circle}`}>
                                                    <div className={`${FormModule.red_circle}`}></div>
                                                </div>
                                            </div>
                                            <div className={`${FormModule.outer_circle1}`}>
                                                <div className={`${FormModule.inner_circle1}`}>
                                                    <div className={`${FormModule.red_circle1}`}></div>
                                                </div>
                                            </div>
                                            <div className='ms-3'>
                                                Primary diagnosis
                                            </div>
                                        </div>

                                    </li>
                                    <li draggable="true" className={`${FormModule.inputField}`} label="Allergies" onClick={Allergies} id='AllergiesSection'>
                                        <span className={`${FormModule.abc} `}>abc</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Allergies</p></li>

                                    <li label="PreviouslyTriedMedication" draggable="true" className={`${FormModule.inputField}`} onClick={PreviouslyTriedMedication} id='PreviouslyTriedMedicationSection'>
                                        <span className={`${FormModule.abc} `}>abc</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Previously tried medication</p></li>

                                    <li label="PatientWeight" draggable="true" className={`${FormModule.inputField}`} onClick={PatientWeight} id='PatientWeightSection'>
                                        <span className={`${FormModule.abc} `}>123</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Patient weight</p></li>

                                    <li label="PatientHeight" draggable="true" className={`${FormModule.inputField}`} onClick={PatientHeight} id='PatientHeightSection'>
                                        <span className={`${FormModule.abc} `}>123</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Patient height</p></li>

                                    <li label="SupportingDocumentation" draggable="true" className={`${FormModule.inputField}`} onClick={SupportingDocumentation} id='SupportingDocumentationSection'>
                                        <span className={`${FormModule.abc} `}>abc</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}>Supporting documentation</p></li>

                                </ul>
                            </div>
                        </Collapse>


                        {/* ***** section 6  ******* */}

                        <div className={`align-items-center d-flex mt-4 p-2 handPointer`} style={{ backgroundColor: open ? '#F3F1F5' : 'initial', borderBottom: open ? '1.5px solid #8A7BA7' : 'initial', boxShadow: open ? '0px 1px 2px rgba(0, 0, 0, 0.15)' : 'initial' }}
                            onClick={handleCollapse6} aria-controls="collapse6" aria-expanded={openCollapse6}>
                            {openCollapse6 && (
                                <FontAwesomeIcon className='grey' icon={faChevronDown} />
                            )}

                            {!openCollapse6 && (
                                <FontAwesomeIcon className='grey' icon={faChevronRight} />
                            )}
                            <span className={`ms-3 grey`}>Prescription information</span>
                        </div>

                        <Collapse in={openCollapse6}>
                            <div id="collapse6">
                                <ul id="frmb-0-control-box" className={` ${FormModule.formcontrol} mt-3 ms-3`}>

                                    <li label="Script" draggable="true" className={`${FormModule.inputField}`} onClick={Script} id='ScriptSection'>
                                        <div className={`${FormModule.circle_container} `}>
                                            <div className={`${FormModule.outer_circle}`}>
                                                <div className={`${FormModule.inner_circle}`}>
                                                    <div className={`${FormModule.red_circle}`}></div>
                                                </div>
                                            </div>
                                            <div className={`${FormModule.outer_circle1}`}>
                                                <div className={`${FormModule.inner_circle1}`}>
                                                    <div className={`${FormModule.red_circle1}`}></div>
                                                </div>
                                            </div>
                                            <div className='ms-3'>
                                                Script
                                            </div>
                                        </div>

                                    </li>
                                    <li label="Pharmacy" draggable="true" className={`${FormModule.inputField}`} onClick={Pharmacy} id='PharmacySection'>
                                        <div className={`${FormModule.circle_container} `}>
                                            <div className={`${FormModule.outer_circle}`}>
                                                <div className={`${FormModule.inner_circle}`}>
                                                    <div className={`${FormModule.red_circle}`}></div>
                                                </div>
                                            </div>
                                            <div className={`${FormModule.outer_circle1}`}>
                                                <div className={`${FormModule.inner_circle1}`}>
                                                    <div className={`${FormModule.red_circle1}`}></div>
                                                </div>
                                            </div>
                                            <div className='ms-3'>
                                                Pharmacy
                                            </div>
                                        </div>
                                    </li>

                                    <li label="DispenseAsWritten" draggable="true" className={`${FormModule.inputField}`} onClick={DispenseAsWritten} id='DispenseAsWrittenSection'>
                                        <span className={`${FormModule.abc} `}>abc</span><p className={`${FormModule.inputField} mb-1 ms-5 ps-4 pt-2`}> Dispense as written </p></li>


                                    <li label="DeliveryInformation" draggable="true" className={`${FormModule.inputField}`} onClick={DeliveryInformation} id='DeliveryInformationSection'>
                                        <div className={`${FormModule.circle_container} `}>
                                            <div className={`${FormModule.outer_circle}`}>
                                                <div className={`${FormModule.inner_circle}`}>
                                                    <div className={`${FormModule.red_circle}`}></div>
                                                </div>
                                            </div>
                                            <div className={`${FormModule.outer_circle1}`}>
                                                <div className={`${FormModule.inner_circle1}`}>
                                                    <div className={`${FormModule.red_circle1}`}></div>
                                                </div>
                                            </div>
                                            <div className='ms-3'>
                                                Delivery information
                                            </div>
                                        </div>
                                    </li>
                                </ul>
                            </div>
                        </Collapse>



                    </div>
                </div>

                <div className={`col-md-8 pt-3`}>

                    <div id='input-cont' className={`${FormModule.centerField} ${FormModule.dropContainer} `} ref={containerRef}>

                    </div>
                </div>
            </div>
        </div>
    )

}
export default FormBuilder
