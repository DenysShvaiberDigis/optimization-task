Task:

Improve the following component’s code. Reduce number of requests & StudentsTable total update time.
fetchStudentData, fetchSchoolData & fetchLegalguardianData can only fetch one entity per request, we don’t have control over remote API
Please, abstain from using help of neural networks - we’re evaluating your programming skill.

```js
import StudentsPicker from '../components/StudentsPicker';
import StudentsTable from '../components/StudentsTable';
import { fetchStudentData, fetchSchoolData, fetchLegalguardianData } from '../utils';
import { useState } from 'react';


const studentsDataComponent = () => {
  const [studentsData, setStudentsData] = useState([]);
  const [schoolsData, setSchoolsData] = useState([]);
  const [legalguardiansData, setLegalguardiansData] = useState([]);


  const onStudentsPick = async (studentIds) => {
    for (const studentId of studentIds) {
      const studentData = await fetchStudentData(studentId);
      setStudentsData([...studentsData, studentData]);
      for (const student of studentData) {
          const { schoolId, legalguardianId } = student;
          const schoolData = await fetchSchoolData(schoolId);
          setSchoolsData([...schoolsData, schoolData]);
          const legalguardianData = await fetchLegalguardianData(legalguardianId);
          setLegalguardiansData([...legalguardiansData, legalguardianData]);
      }
    }
  };


  return (
    <>
      <StudentsPicker onPickHandler={onStudentsPick} />
      <StudentsTable
        studentsData={studentsData}
        schoolsData={schoolsData}
        LegalguardiansData={legalguardiansData}
      />
    </>
  );
};


export default studentsDataComponent;
```