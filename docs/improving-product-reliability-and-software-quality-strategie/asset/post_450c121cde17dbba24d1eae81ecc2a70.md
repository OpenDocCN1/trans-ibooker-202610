
// int ChangeOrientation(int angle)
// changes the robot's orientation by some angle
// angle = 0 to 360 degrees
// returns success or an error code
// see enums for return values

enum return_code { Success, Illegal_angle, Rotation_Failure };
enum priority { Emergency, Alert, Critical, Error, Warning, Notice, Informational, Debug };

int ChangeOrientation( int angle )
{
    int rc;
    double wheelAmount;

    // debug log rotate() entry
#ifdef DEBUG
log(Debug, get_system_time(), id->sin_addr, "enter ChangeOrientation(), turn angle = %d", angle);
#endif /* DEBUG */

    // check that angle is legal, must be between 0 and 360
    // if the angle is 0 or 360, then do nothing
    if (angle > 0 && angle < 360) {
        // calculate how much to turn the wheels
        // if left turn (i.e. angle is > 180) then calculate negative turn value
        if ( angle <= 180)
            wheelAmount = (angle / 360) * ((trackWidth * PI) / wheelCircumference);
        else {
            angle -= 180;
            wheelAmount = (angle / 360) * ((trackWidth * PI) / wheelCircumference);
            wheelAmount *= -1;
        }

        // PointTurn() does a point turn where
        // the left wheel turns forward and the right wheel turns in reverse by wheelAmount
        rc = PointTurn(wheelAmount);

        // handle failure of PointTurn()
        if (rc > 0) {
            log(Error, get_system_time(), myIpAddr, "PointTurn() failed with error %d", rc);
            return Rotation_Failure;
        }
    } else if (angle < 0 || angle > 360) {      // if angle is illegal, log it and return a failure
        log(Error, get_system_time(), id->sin_addr, "illegal angle specified %d", angle);
        return Illegal_angle;
    }

#ifdef DEBUG
log(Debug, get_system_time(), id->sin_addr, "exiting rotate() successfully");
#endif /* DEBUG */
    return Success;
}

## Figure 22.3 Sample log code.

management of all components. New standards are emerging for monitoring and tracking of equipment for various industries, e.g. there is an emerging standard (VDI 5600 for Factory 4.0) for equipment and sensors in automated factories. If your product is expected to be deployed in a heterogeneous environment, then it is suggested that you use whatever standard logging protocols and formats are appropriate for that environment if they have been defined.

```
<7> Mar 12 12:27:00.003 198.162.1.33 SensorCheck(), forward sensor: obstruction identified
<7> Mar 12 12:27:00.003 198.162.1.33 SensorCheck(), right sensor: clear
<7> Mar 12 12:27:00.003 198.162.1.33 SensorCheck(), calling ChangeOrientation(90)
<7> Mar 12 12:27:00.004 198.162.1.33 enter ChangeOrientation(), turn angle = 90
<7> Mar 12 12:27:00.004 198.162.1.33 enter PointTurn(), wheelAmount = 0.5
<7> Mar 12 12:27:00.006 198.162.1.33 PointTurn() exiting successfully
<7> Mar 12 12:27:00.006 198.162.1.33 ChangeOrientation() exiting successfully
<7> Mar 12 12:27:00.007 198.162.1.33 SensorCheck() exiting successfully
```

## Figure 22.4 Example log file extract.

## Further Reading

J. Engblom, G. Girard, B. Werner, Testing Embedded Software using Simulated Hardware, ERTS Conference 2006, 2006.
Gerhards, R. (2009). RFC 5424 – The Syslog Protocol. IETF Trust.
Júnior, J.C.V.S., Brito, A.V., and Nascimento, T.P. (2015). Verification of embedded system designs through hardware-software co-simulation. International Journal of Information and Electronics Engineering 5 (1).
King, P.J. and Copp, D.G. (2008). Hardware in the loop for automotive vehicle control systems development and testing. Measurement and Control Journal 39 (1).
Kohl, S. and Jegminat, D. (2005). How to Do Hardware-In-The-Loop Simulation Right. SAE International.
The CERT Division of the Software Engineering Institute (SEI), https://www.sei.cmu.edu/about/divisions/cert/index.cfm
The Open Web Application Security Project, https://www.owasp.org
The National Cybersecurity and Communications Integration Center's (NCCIC), https://www.us-cert.gov

## 23 Applying Software Quality Procedures

Once all the techniques are understood, they are put together in a process that enables the software organization to produce high-quality software. There are two generic types of processes that the organization should employ. The first is a software development process. The second is an organizational process improvement process. The software development process is the set of metrics and techniques used to deliver a specific software project with adequate quality. The organization process improvement process allows the group to improve software quality from release to release.

The overview of the process during a project has seven elements:

- 1. Develop requirements, including quality goals.
- 2. Size the work.
- 3. Create a defect run chart showing where the software is against the quality goals.
- 4. Use failure modes and effects analysis (FMEAs), reviews, inspections, and defensive programming techniques to avoid defect injection.
- 5. Employ the various testing techniques to discover defects.
- 6. Measure defects discovered during the project and keep the defect run chart up to date.
- 7. Use the defect run chart to determine when the software quality goals have been achieved.

The generic process described above is usable with both waterfall and Agile software life cycles. During waterfall life cycles, these activities map pretty cleanly into the various stages of the project. In an Agile process, each of these activities could be used during a sprint. However, in Agile, some of the activities might be used in between sprints for retrospectives, or applied to an entire epic. For instance, each sprint would have sizing, requirements, reviews, code inspections, and testing. Each sprint could have a defect run chart to track quality for that spring. However, the defect run chart might also be applied to an epic rather than a sprint. If each sprint will be released to a customer, it would be more appropriate to have a defect run chart per sprint. On the other hand, if an epic represents the actual customer release, then the defect run chart might be better applied to the epic. Likewise, the process improvement methods, such as root cause analysis (RCA) and FMEAs, might fit into the retrospective held between sprints. However, these mechanisms typically would take several days to perform well, so it is perfectly valid

to apply them to the epic and perform them outside of the sprint process as part of an overall process improvement process. Another option for the process improvement mechanisms using Agile is to create user stories that cover them and actually perform tasks such as RCA and FMEAs as deliverables during various sprints.

Long-term quality improvement processes generally do the following:

- 1. Set software quality goals primarily using escape defect rates.
- 2. Measure the escaped defect rate for each release.
- 3. In between releases (or as orthogonal activities during a release), use analytic techniques such as RCA and FMEAs to identify areas of weakness with the technology, training, and processes.
- 4. Address the weaknesses identified in the previous step.
- 5. Repeat this procedure.

### 23.1 Using Defect Model to Create Defect Run Chart

Start with the simplest type of defect model where only the total number of injected defects are estimated. To create this simple model, calculate the total number of defects expected to be injected in the project by multiplying defect density estimate with the lines of code (LOC) size estimate.

It is most accurate to use your development group’s historically measured defect density data for this calculation. If you do not have historical data from your organization, you can use industry data as discussed in Section 9.3 on defect density; i.e. 10 defects per KLOC for low-level languages and 20 defects per KLOC for high-level languages.

Defects will not be injected at a steady rate during the course of the project. Nor will the defects be discovered at a steady rate. Defects will be injected during each activity in the project, i.e. requirements development, design, coding, and even while fixing other defects. Defects will be discovered during reviews, code inspections, and during each of the testing activities. Typically, most of the defects will be injected during coding, and most of the defects will be discovered and fixed during testing. A very sophisticated defect model might estimate the number of defects injected, discovered, and fixed over the course of time during the project, perhaps even taking into account the number of engineers developing, testing and fixing code at any given point in time. For organizations just starting out with defect run charts, it is sufficient to just create a simple burn up chart that plots the total number of defects expected and progress toward finding them as shown in Section 9.5 on defect run charts.

### 23.2 Using Defect Run Chart to Know When You Have Achieved the Quality Target

All defects found during the software development must be counted. This should include defects found during reviews, inspections, and developer testing in addition to those found during the traditional software quality assurance (SQA) type testing. Likewise, each defect that is fixed must be counted. Having accurate defect discovery and closure
