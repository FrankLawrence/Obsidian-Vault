```XML
<StudyStructure>
  <APIVrs>1.01.05</APIVrs>
  <ElmtDefs>
    <Struct Name="Study">
      <Struct Name="ProtocolSection" DEDLink="https://prsinfo.clinicaltrials.gov/definitions.html">
        <Struct Name="IdentificationModule" DEDLink="https://prsinfo.clinicaltrials.gov/definitions.html#identification">
          <Field Name="NCTId"/>
          <List Name="NCTIdAliasList">
            <Field Name="NCTIdAlias"/>
          </List>
          <Struct Name="OrgStudyIdInfo">
            <Field Name="OrgStudyId"/>
            <Field Name="OrgStudyIdType"/>
            <Field Name="OrgStudyIdDomain"/>
            <Field Name="OrgStudyIdLink"/>
          </Struct>
          <List Name="SecondaryIdInfoList">
            <Struct Name="SecondaryIdInfo">
              <Field Name="SecondaryId"/>
              <Field Name="SecondaryIdType"/>
              <Field Name="SecondaryIdDomain"/>
              <Field Name="SecondaryIdLink"/>
            </Struct>
          </List>
          <Struct Name="Organization">
            <Field Name="OrgFullName"/>
            <Field Name="OrgClass"/>
          </Struct>
          <Field Name="BriefTitle"/>
          <Field Name="OfficialTitle"/>
          <Field Name="Acronym"/>
        </Struct>
        <Struct Name="StatusModule" DEDLink="https://prsinfo.clinicaltrials.gov/definitions.html#status">
          <Field Name="StatusVerifiedDate"/>
          <Field Name="OverallStatus"/>
          <Field Name="LastKnownStatus"/>
          <Field Name="DelayedPosting"/>
          <Field Name="WhyStopped"/>
          <Struct Name="ExpandedAccessInfo">
            <Field Name="HasExpandedAccess"/>
            <Field Name="ExpandedAccessNCTId"/>
            <Field Name="ExpandedAccessStatusForNCTId"/>
          </Struct>
          <Struct Name="StartDateStruct">
            <Field Name="StartDate"/>
            <Field Name="StartDateType"/>
          </Struct>
          <Struct Name="PrimaryCompletionDateStruct">
            <Field Name="PrimaryCompletionDate"/>
            <Field Name="PrimaryCompletionDateType"/>
          </Struct>
          <Struct Name="CompletionDateStruct">
            <Field Name="CompletionDate"/>
            <Field Name="CompletionDateType"/>
          </Struct>
          <Field Name="StudyFirstSubmitDate"/>
          <Field Name="StudyFirstSubmitQCDate"/>
          <Struct Name="StudyFirstPostDateStruct">
            <Field Name="StudyFirstPostDate"/>
            <Field Name="StudyFirstPostDateType"/>
          </Struct>
          <Field Name="ResultsFirstSubmitDate"/>
          <Field Name="ResultsFirstSubmitQCDate"/>
          <Struct Name="ResultsFirstPostDateStruct">
            <Field Name="ResultsFirstPostDate"/>
            <Field Name="ResultsFirstPostDateType"/>
          </Struct>
          <Struct Name="ResultsFirstPostedQCCommentsDateStruct">
            <Field Name="ResultsFirstPostedQCCommentsDate"/>
            <Field Name="ResultsFirstPostedQCCommentsDateType"/>
          </Struct>
          <Field Name="DispFirstSubmitDate"/>
          <Field Name="DispFirstSubmitQCDate"/>
          <Struct Name="DispFirstPostDateStruct">
            <Field Name="DispFirstPostDate"/>
            <Field Name="DispFirstPostDateType"/>
          </Struct>
          <Field Name="LastUpdateSubmitDate"/>
          <Struct Name="LastUpdatePostDateStruct">
            <Field Name="LastUpdatePostDate"/>
            <Field Name="LastUpdatePostDateType"/>
          </Struct>
        </Struct>
        <Struct Name="SponsorCollaboratorsModule" DEDLink="https://prsinfo.clinicaltrials.gov/definitions.html#sponsors">
          <Struct Name="ResponsibleParty">
            <Field Name="ResponsiblePartyType"/>
            <Field Name="ResponsiblePartyInvestigatorFullName"/>
            <Field Name="ResponsiblePartyInvestigatorTitle"/>
            <Field Name="ResponsiblePartyInvestigatorAffiliation"/>
            <Field Name="ResponsiblePartyOldNameTitle"/>
            <Field Name="ResponsiblePartyOldOrganization"/>
          </Struct>
          <Struct Name="LeadSponsor">
            <Field Name="LeadSponsorName"/>
            <Field Name="LeadSponsorClass"/>
          </Struct>
          <List Name="CollaboratorList">
            <Struct Name="Collaborator">
              <Field Name="CollaboratorName"/>
              <Field Name="CollaboratorClass"/>
            </Struct>
          </List>
        </Struct>
        <Struct Name="OversightModule" DEDLink="https://prsinfo.clinicaltrials.gov/definitions.html#oversight">
          <Field Name="OversightHasDMC"/>
          <Field Name="IsFDARegulatedDrug"/>
          <Field Name="IsFDARegulatedDevice"/>
          <Field Name="IsUnapprovedDevice"/>
          <Field Name="IsPPSD"/>
          <Field Name="IsUSExport"/>
          <Field Name="FDAAA801Violation"/>
        </Struct>
        <Struct Name="DescriptionModule" DEDLink="https://prsinfo.clinicaltrials.gov/definitions.html#description">
          <Field Name="BriefSummary"/>
          <Field Name="DetailedDescription"/>
        </Struct>
        <Struct Name="ConditionsModule" DEDLink="https://prsinfo.clinicaltrials.gov/definitions.html#Conditions">
          <List Name="ConditionList">
            <Field Name="Condition"/>
          </List>
          <List Name="KeywordList">
            <Field Name="Keyword"/>
          </List>
        </Struct>
        <Struct Name="DesignModule" DEDLink="https://prsinfo.clinicaltrials.gov/definitions.html#StudyDesign">
          <Field Name="StudyType" DEDLink="https://prsinfo.clinicaltrials.gov/definitions.html#StudyType"/>
          <Struct Name="ExpandedAccessTypes">
            <Field Name="ExpAccTypeIndividual"/>
            <Field Name="ExpAccTypeIntermediate"/>
            <Field Name="ExpAccTypeTreatment"/>
          </Struct>
          <Field Name="PatientRegistry"/>
          <Field Name="TargetDuration"/>
          <List Name="PhaseList">
            <Field Name="Phase"/>
          </List>
          <Struct Name="DesignInfo">
            <Field Name="DesignAllocation"/>
            <Field Name="DesignInterventionModel"/>
            <Field Name="DesignInterventionModelDescription"/>
            <Field Name="DesignPrimaryPurpose"/>
            <List Name="DesignObservationalModelList">
              <Field Name="DesignObservationalModel"/>
            </List>
            <List Name="DesignTimePerspectiveList">
              <Field Name="DesignTimePerspective"/>
            </List>
            <Struct Name="DesignMaskingInfo">
              <Field Name="DesignMasking"/>
              <Field Name="DesignMaskingDescription"/>
              <List Name="DesignWhoMaskedList">
                <Field Name="DesignWhoMasked"/>
              </List>
            </Struct>
          </Struct>
          <Struct Name="BioSpec">
            <Field Name="BioSpecRetention"/>
            <Field Name="BioSpecDescription"/>
          </Struct>
          <Struct Name="EnrollmentInfo">
            <Field Name="EnrollmentCount"/>
            <Field Name="EnrollmentType"/>
          </Struct>
        </Struct>
        <Struct Name="ArmsInterventionsModule" DEDLink="https://prsinfo.clinicaltrials.gov/definitions.html#ArmsGroupsInterventions">
          <List Name="ArmGroupList">
            <Struct Name="ArmGroup">
              <Field Name="ArmGroupLabel"/>
              <Field Name="ArmGroupType"/>
              <Field Name="ArmGroupDescription"/>
              <List Name="ArmGroupInterventionList">
                <Field Name="ArmGroupInterventionName"/>
              </List>
            </Struct>
          </List>
          <List Name="InterventionList">
            <Struct Name="Intervention">
              <Field Name="InterventionType"/>
              <Field Name="InterventionName"/>
              <Field Name="InterventionDescription"/>
              <List Name="InterventionArmGroupLabelList">
                <Field Name="InterventionArmGroupLabel"/>
              </List>
              <List Name="InterventionOtherNameList">
                <Field Name="InterventionOtherName"/>
              </List>
            </Struct>
          </List>
        </Struct>
        <Struct Name="OutcomesModule" DEDLink="https://prsinfo.clinicaltrials.gov/definitions.html#Outcomes">
          <List Name="PrimaryOutcomeList">
            <Struct Name="PrimaryOutcome">
              <Field Name="PrimaryOutcomeMeasure"/>
              <Field Name="PrimaryOutcomeDescription"/>
              <Field Name="PrimaryOutcomeTimeFrame"/>
            </Struct>
          </List>
          <List Name="SecondaryOutcomeList">
            <Struct Name="SecondaryOutcome">
              <Field Name="SecondaryOutcomeMeasure"/>
              <Field Name="SecondaryOutcomeDescription"/>
              <Field Name="SecondaryOutcomeTimeFrame"/>
            </Struct>
          </List>
          <List Name="OtherOutcomeList">
            <Struct Name="OtherOutcome">
              <Field Name="OtherOutcomeMeasure"/>
              <Field Name="OtherOutcomeDescription"/>
              <Field Name="OtherOutcomeTimeFrame"/>
            </Struct>
          </List>
        </Struct>
        <Struct Name="EligibilityModule" DEDLink="https://prsinfo.clinicaltrials.gov/definitions.html#Eligibility">
          <Field Name="EligibilityCriteria"/>
          <Field Name="HealthyVolunteers"/>
          <Field Name="Gender"/>
          <Field Name="GenderBased"/>
          <Field Name="GenderDescription"/>
          <Field Name="MinimumAge"/>
          <Field Name="MaximumAge"/>
          <List Name="StdAgeList">
            <Field Name="StdAge"/>
          </List>
          <Field Name="StudyPopulation"/>
          <Field Name="SamplingMethod"/>
        </Struct>
        <Struct Name="ContactsLocationsModule" DEDLink="https://prsinfo.clinicaltrials.gov/definitions.html#Locations">
          <List Name="CentralContactList">
            <Struct Name="CentralContact">
              <Field Name="CentralContactName"/>
              <Field Name="CentralContactRole"/>
              <Field Name="CentralContactPhone"/>
              <Field Name="CentralContactPhoneExt"/>
              <Field Name="CentralContactEMail"/>
            </Struct>
          </List>
          <List Name="OverallOfficialList">
            <Struct Name="OverallOfficial">
              <Field Name="OverallOfficialName"/>
              <Field Name="OverallOfficialAffiliation"/>
              <Field Name="OverallOfficialRole"/>
            </Struct>
          </List>
          <List Name="LocationList">
            <Struct Name="Location">
              <Field Name="LocationFacility"/>
              <Field Name="LocationStatus"/>
              <Field Name="LocationCity"/>
              <Field Name="LocationState"/>
              <Field Name="LocationZip"/>
              <Field Name="LocationCountry"/>
              <List Name="LocationContactList">
                <Struct Name="LocationContact">
                  <Field Name="LocationContactName"/>
                  <Field Name="LocationContactRole"/>
                  <Field Name="LocationContactPhone"/>
                  <Field Name="LocationContactPhoneExt"/>
                  <Field Name="LocationContactEMail"/>
                </Struct>
              </List>
            </Struct>
          </List>
        </Struct>
        <Struct Name="ReferencesModule" DEDLink="https://prsinfo.clinicaltrials.gov/definitions.html#References">
          <List Name="ReferenceList">
            <Struct Name="Reference">
              <Field Name="ReferencePMID"/>
              <Field Name="ReferenceType"/>
              <Field Name="ReferenceCitation"/>
              <List Name="RetractionList">
                <Struct Name="Retraction">
                  <Field Name="RetractionPMID"/>
                  <Field Name="RetractionSource"/>
                </Struct>
              </List>
            </Struct>
          </List>
          <List Name="SeeAlsoLinkList">
            <Struct Name="SeeAlsoLink">
              <Field Name="SeeAlsoLinkLabel"/>
              <Field Name="SeeAlsoLinkURL"/>
            </Struct>
          </List>
          <List Name="AvailIPDList">
            <Struct Name="AvailIPD">
              <Field Name="AvailIPDId"/>
              <Field Name="AvailIPDType"/>
              <Field Name="AvailIPDURL"/>
              <Field Name="AvailIPDComment"/>
            </Struct>
          </List>
        </Struct>
        <Struct Name="IPDSharingStatementModule" DEDLink="https://prsinfo.clinicaltrials.gov/definitions.html#IPDSharing">
          <Field Name="IPDSharing"/>
          <Field Name="IPDSharingDescription"/>
          <List Name="IPDSharingInfoTypeList">
            <Field Name="IPDSharingInfoType"/>
          </List>
          <Field Name="IPDSharingTimeFrame"/>
          <Field Name="IPDSharingAccessCriteria"/>
          <Field Name="IPDSharingURL"/>
        </Struct>
      </Struct>
      <Struct Name="ResultsSection" DEDLink="https://prsinfo.clinicaltrials.gov/results_definitions.html">
        <Struct Name="ParticipantFlowModule" DEDLink="https://prsinfo.clinicaltrials.gov/results_definitions.html#Result_ParticipantFlow">
          <Field Name="FlowPreAssignmentDetails"/>
          <Field Name="FlowRecruitmentDetails"/>
          <Field Name="FlowTypeUnitsAnalyzed"/>
          <List Name="FlowGroupList">
            <Struct Name="FlowGroup">
              <Field Name="FlowGroupId"/>
              <Field Name="FlowGroupTitle"/>
              <Field Name="FlowGroupDescription"/>
            </Struct>
          </List>
          <List Name="FlowPeriodList">
            <Struct Name="FlowPeriod">
              <Field Name="FlowPeriodTitle"/>
              <List Name="FlowMilestoneList">
                <Struct Name="FlowMilestone">
                  <Field Name="FlowMilestoneType"/>
                  <Field Name="FlowMilestoneComment"/>
                  <List Name="FlowAchievementList">
                    <Struct Name="FlowAchievement">
                      <Field Name="FlowAchievementGroupId"/>
                      <Field Name="FlowAchievementComment"/>
                      <Field Name="FlowAchievementNumSubjects"/>
                      <Field Name="FlowAchievementNumUnits"/>
                    </Struct>
                  </List>
                </Struct>
              </List>
              <List Name="FlowDropWithdrawList">
                <Struct Name="FlowDropWithdraw">
                  <Field Name="FlowDropWithdrawType"/>
                  <Field Name="FlowDropWithdrawComment"/>
                  <List Name="FlowReasonList">
                    <Struct Name="FlowReason">
                      <Field Name="FlowReasonGroupId"/>
                      <Field Name="FlowReasonComment"/>
                      <Field Name="FlowReasonNumSubjects"/>
                      <Field Name="FlowReasonNumUnits"/>
                    </Struct>
                  </List>
                </Struct>
              </List>
            </Struct>
          </List>
        </Struct>
        <Struct Name="BaselineCharacteristicsModule" DEDLink="https://prsinfo.clinicaltrials.gov/results_definitions.html#Result_Baseline">
          <Field Name="BaselinePopulationDescription"/>
          <Field Name="BaselineTypeUnitsAnalyzed"/>
          <List Name="BaselineGroupList">
            <Struct Name="BaselineGroup">
              <Field Name="BaselineGroupId"/>
              <Field Name="BaselineGroupTitle"/>
              <Field Name="BaselineGroupDescription"/>
            </Struct>
          </List>
          <List Name="BaselineDenomList">
            <Struct Name="BaselineDenom">
              <Field Name="BaselineDenomUnits"/>
              <List Name="BaselineDenomCountList">
                <Struct Name="BaselineDenomCount">
                  <Field Name="BaselineDenomCountGroupId"/>
                  <Field Name="BaselineDenomCountValue"/>
                </Struct>
              </List>
            </Struct>
          </List>
          <List Name="BaselineMeasureList">
            <Struct Name="BaselineMeasure">
              <Field Name="BaselineMeasureTitle"/>
              <Field Name="BaselineMeasureDescription"/>
              <Field Name="BaselineMeasurePopulationDescription"/>
              <Field Name="BaselineMeasureParamType"/>
              <Field Name="BaselineMeasureDispersionType"/>
              <Field Name="BaselineMeasureUnitOfMeasure"/>
              <Field Name="BaselineMeasureCalculatePct"/>
              <Field Name="BaselineMeasureDenomUnitsSelected"/>
              <List Name="BaselineMeasureDenomList">
                <Struct Name="BaselineMeasureDenom">
                  <Field Name="BaselineMeasureDenomUnits"/>
                  <List Name="BaselineMeasureDenomCountList">
                    <Struct Name="BaselineMeasureDenomCount">
                      <Field Name="BaselineMeasureDenomCountGroupId"/>
                      <Field Name="BaselineMeasureDenomCountValue"/>
                    </Struct>
                  </List>
                </Struct>
              </List>
              <List Name="BaselineClassList">
                <Struct Name="BaselineClass">
                  <Field Name="BaselineClassTitle"/>
                  <List Name="BaselineClassDenomList">
                    <Struct Name="BaselineClassDenom">
                      <Field Name="BaselineClassDenomUnits"/>
                      <List Name="BaselineClassDenomCountList">
                        <Struct Name="BaselineClassDenomCount">
                          <Field Name="BaselineClassDenomCountGroupId"/>
                          <Field Name="BaselineClassDenomCountValue"/>
                        </Struct>
                      </List>
                    </Struct>
                  </List>
                  <List Name="BaselineCategoryList">
                    <Struct Name="BaselineCategory">
                      <Field Name="BaselineCategoryTitle"/>
                      <List Name="BaselineMeasurementList">
                        <Struct Name="BaselineMeasurement">
                          <Field Name="BaselineMeasurementGroupId"/>
                          <Field Name="BaselineMeasurementValue"/>
                          <Field Name="BaselineMeasurementSpread"/>
                          <Field Name="BaselineMeasurementLowerLimit"/>
                          <Field Name="BaselineMeasurementUpperLimit"/>
                          <Field Name="BaselineMeasurementComment"/>
                        </Struct>
                      </List>
                    </Struct>
                  </List>
                </Struct>
              </List>
            </Struct>
          </List>
        </Struct>
        <Struct Name="OutcomeMeasuresModule" DEDLink="https://prsinfo.clinicaltrials.gov/results_definitions.html#Result_Outcome_Measure">
          <List Name="OutcomeMeasureList">
            <Struct Name="OutcomeMeasure">
              <Field Name="OutcomeMeasureType"/>
              <Field Name="OutcomeMeasureTitle"/>
              <Field Name="OutcomeMeasureDescription"/>
              <Field Name="OutcomeMeasurePopulationDescription"/>
              <Field Name="OutcomeMeasureReportingStatus"/>
              <Field Name="OutcomeMeasureAnticipatedPostingDate"/>
              <Field Name="OutcomeMeasureParamType"/>
              <Field Name="OutcomeMeasureDispersionType"/>
              <Field Name="OutcomeMeasureUnitOfMeasure"/>
              <Field Name="OutcomeMeasureCalculatePct"/>
              <Field Name="OutcomeMeasureTimeFrame"/>
              <Field Name="OutcomeMeasureTypeUnitsAnalyzed"/>
              <Field Name="OutcomeMeasureDenomUnitsSelected"/>
              <List Name="OutcomeGroupList">
                <Struct Name="OutcomeGroup">
                  <Field Name="OutcomeGroupId"/>
                  <Field Name="OutcomeGroupTitle"/>
                  <Field Name="OutcomeGroupDescription"/>
                </Struct>
              </List>
              <List Name="OutcomeDenomList">
                <Struct Name="OutcomeDenom">
                  <Field Name="OutcomeDenomUnits"/>
                  <List Name="OutcomeDenomCountList">
                    <Struct Name="OutcomeDenomCount">
                      <Field Name="OutcomeDenomCountGroupId"/>
                      <Field Name="OutcomeDenomCountValue"/>
                    </Struct>
                  </List>
                </Struct>
              </List>
              <List Name="OutcomeClassList">
                <Struct Name="OutcomeClass">
                  <Field Name="OutcomeClassTitle"/>
                  <List Name="OutcomeClassDenomList">
                    <Struct Name="OutcomeClassDenom">
                      <Field Name="OutcomeClassDenomUnits"/>
                      <List Name="OutcomeClassDenomCountList">
                        <Struct Name="OutcomeClassDenomCount">
                          <Field Name="OutcomeClassDenomCountGroupId"/>
                          <Field Name="OutcomeClassDenomCountValue"/>
                        </Struct>
                      </List>
                    </Struct>
                  </List>
                  <List Name="OutcomeCategoryList">
                    <Struct Name="OutcomeCategory">
                      <Field Name="OutcomeCategoryTitle"/>
                      <List Name="OutcomeMeasurementList">
                        <Struct Name="OutcomeMeasurement">
                          <Field Name="OutcomeMeasurementGroupId"/>
                          <Field Name="OutcomeMeasurementValue"/>
                          <Field Name="OutcomeMeasurementSpread"/>
                          <Field Name="OutcomeMeasurementLowerLimit"/>
                          <Field Name="OutcomeMeasurementUpperLimit"/>
                          <Field Name="OutcomeMeasurementComment"/>
                        </Struct>
                      </List>
                    </Struct>
                  </List>
                </Struct>
              </List>
              <List Name="OutcomeAnalysisList" DEDLink="https://prsinfo.clinicaltrials.gov/results_definitions.html#Result_Outcome_Analysis">
                <Struct Name="OutcomeAnalysis">
                  <List Name="OutcomeAnalysisGroupIdList">
                    <Field Name="OutcomeAnalysisGroupId"/>
                  </List>
                  <Field Name="OutcomeAnalysisGroupDescription"/>
                  <Field Name="OutcomeAnalysisTestedNonInferiority"/>
                  <Field Name="OutcomeAnalysisNonInferiorityType"/>
                  <Field Name="OutcomeAnalysisNonInferiorityComment"/>
                  <Field Name="OutcomeAnalysisPValue"/>
                  <Field Name="OutcomeAnalysisPValueComment"/>
                  <Field Name="OutcomeAnalysisStatisticalMethod"/>
                  <Field Name="OutcomeAnalysisStatisticalComment"/>
                  <Field Name="OutcomeAnalysisParamType"/>
                  <Field Name="OutcomeAnalysisParamValue"/>
                  <Field Name="OutcomeAnalysisCIPctValue"/>
                  <Field Name="OutcomeAnalysisCINumSides"/>
                  <Field Name="OutcomeAnalysisCILowerLimit"/>
                  <Field Name="OutcomeAnalysisCIUpperLimit"/>
                  <Field Name="OutcomeAnalysisCILowerLimitComment"/>
                  <Field Name="OutcomeAnalysisCIUpperLimitComment"/>
                  <Field Name="OutcomeAnalysisDispersionType"/>
                  <Field Name="OutcomeAnalysisDispersionValue"/>
                  <Field Name="OutcomeAnalysisEstimateComment"/>
                  <Field Name="OutcomeAnalysisOtherAnalysisDescription"/>
                </Struct>
              </List>
            </Struct>
          </List>
        </Struct>
        <Struct Name="AdverseEventsModule" DEDLink="https://prsinfo.clinicaltrials.gov/results_definitions.html#Result_AdverseEvents">
          <Field Name="EventsFrequencyThreshold"/>
          <Field Name="EventsTimeFrame"/>
          <Field Name="EventsDescription"/>
          <List Name="EventGroupList">
            <Struct Name="EventGroup">
              <Field Name="EventGroupId"/>
              <Field Name="EventGroupTitle"/>
              <Field Name="EventGroupDescription"/>
              <Field Name="EventGroupDeathsNumAffected"/>
              <Field Name="EventGroupDeathsNumAtRisk"/>
              <Field Name="EventGroupSeriousNumAffected"/>
              <Field Name="EventGroupSeriousNumAtRisk"/>
              <Field Name="EventGroupOtherNumAffected"/>
              <Field Name="EventGroupOtherNumAtRisk"/>
            </Struct>
          </List>
          <List Name="SeriousEventList">
            <Struct Name="SeriousEvent">
              <Field Name="SeriousEventTerm"/>
              <Field Name="SeriousEventOrganSystem"/>
              <Field Name="SeriousEventSourceVocabulary"/>
              <Field Name="SeriousEventAssessmentType"/>
              <Field Name="SeriousEventNotes"/>
              <List Name="SeriousEventStatsList">
                <Struct Name="SeriousEventStats">
                  <Field Name="SeriousEventStatsGroupId"/>
                  <Field Name="SeriousEventStatsNumEvents"/>
                  <Field Name="SeriousEventStatsNumAffected"/>
                  <Field Name="SeriousEventStatsNumAtRisk"/>
                </Struct>
              </List>
            </Struct>
          </List>
          <List Name="OtherEventList">
            <Struct Name="OtherEvent">
              <Field Name="OtherEventTerm"/>
              <Field Name="OtherEventOrganSystem"/>
              <Field Name="OtherEventSourceVocabulary"/>
              <Field Name="OtherEventAssessmentType"/>
              <Field Name="OtherEventNotes"/>
              <List Name="OtherEventStatsList">
                <Struct Name="OtherEventStats">
                  <Field Name="OtherEventStatsGroupId"/>
                  <Field Name="OtherEventStatsNumEvents"/>
                  <Field Name="OtherEventStatsNumAffected"/>
                  <Field Name="OtherEventStatsNumAtRisk"/>
                </Struct>
              </List>
            </Struct>
          </List>
        </Struct>
        <Struct Name="MoreInfoModule">
          <Field Name="LimitationsAndCaveatsDescription" DEDLink="https://prsinfo.clinicaltrials.gov/results_definitions.html#Result_LimitationsAndCaveats_description"/>
          <Struct Name="CertainAgreement" DEDLink="https://prsinfo.clinicaltrials.gov/results_definitions.html#Result_CertainAgreement">
            <Field Name="AgreementPISponsorEmployee"/>
            <Field Name="AgreementRestrictionType"/>
            <Field Name="AgreementRestrictiveAgreement"/>
            <Field Name="AgreementOtherDetails"/>
          </Struct>
          <Struct Name="PointOfContact" DEDLink="https://prsinfo.clinicaltrials.gov/results_definitions.html#Result_PointOfContact">
            <Field Name="PointOfContactTitle"/>
            <Field Name="PointOfContactOrganization"/>
            <Field Name="PointOfContactEMail"/>
            <Field Name="PointOfContactPhone"/>
            <Field Name="PointOfContactPhoneExt"/>
          </Struct>
        </Struct>
      </Struct>
      <Struct Name="AnnotationSection">
        <Struct Name="AnnotationModule">
          <Struct Name="UnpostedAnnotation">
            <Field Name="UnpostedResponsibleParty"/>
            <List Name="UnpostedEventList">
              <Struct Name="UnpostedEvent">
                <Field Name="UnpostedEventType"/>
                <Field Name="UnpostedEventDate"/>
              </Struct>
            </List>
          </Struct>
        </Struct>
      </Struct>
      <Struct Name="DocumentSection">
        <Struct Name="LargeDocumentModule" DEDLink="https://prsinfo.clinicaltrials.gov/results_definitions.html#DocumentUploadLabel">
          <List Name="LargeDocList">
            <Struct Name="LargeDoc">
              <Field Name="LargeDocTypeAbbrev"/>
              <Field Name="LargeDocHasProtocol"/>
              <Field Name="LargeDocHasSAP"/>
              <Field Name="LargeDocHasICF"/>
              <Field Name="LargeDocLabel"/>
              <Field Name="LargeDocDate"/>
              <Field Name="LargeDocUploadDate"/>
              <Field Name="LargeDocFilename"/>
            </Struct>
          </List>
        </Struct>
      </Struct>
      <Struct Name="DerivedSection">
        <Struct Name="MiscInfoModule">
          <Field Name="VersionHolder"/>
          <List Name="RemovedCountryList">
            <Field Name="RemovedCountry"/>
          </List>
          <Struct Name="SubmissionTracking">
            <List Name="SubmissionInfoList">
              <Struct Name="SubmissionInfo">
                <Field Name="SubmissionReleaseDate"/>
                <Field Name="SubmissionUnreleaseDate"/>
                <Field Name="SubmissionResetDate"/>
                <Field Name="SubmissionMCPReleaseN"/>
              </Struct>
            </List>
          </Struct>
        </Struct>
        <Struct Name="ConditionBrowseModule">
          <List Name="ConditionMeshList">
            <Struct Name="ConditionMesh">
              <Field Name="ConditionMeshId"/>
              <Field Name="ConditionMeshTerm"/>
            </Struct>
          </List>
          <List Name="ConditionAncestorList">
            <Struct Name="ConditionAncestor">
              <Field Name="ConditionAncestorId"/>
              <Field Name="ConditionAncestorTerm"/>
            </Struct>
          </List>
          <List Name="ConditionBrowseLeafList">
            <Struct Name="ConditionBrowseLeaf">
              <Field Name="ConditionBrowseLeafId"/>
              <Field Name="ConditionBrowseLeafName"/>
              <Field Name="ConditionBrowseLeafAsFound"/>
              <Field Name="ConditionBrowseLeafRelevance"/>
            </Struct>
          </List>
          <List Name="ConditionBrowseBranchList">
            <Struct Name="ConditionBrowseBranch">
              <Field Name="ConditionBrowseBranchAbbrev"/>
              <Field Name="ConditionBrowseBranchName"/>
            </Struct>
          </List>
        </Struct>
        <Struct Name="InterventionBrowseModule">
          <List Name="InterventionMeshList">
            <Struct Name="InterventionMesh">
              <Field Name="InterventionMeshId"/>
              <Field Name="InterventionMeshTerm"/>
            </Struct>
          </List>
          <List Name="InterventionAncestorList">
            <Struct Name="InterventionAncestor">
              <Field Name="InterventionAncestorId"/>
              <Field Name="InterventionAncestorTerm"/>
            </Struct>
          </List>
          <List Name="InterventionBrowseLeafList">
            <Struct Name="InterventionBrowseLeaf">
              <Field Name="InterventionBrowseLeafId"/>
              <Field Name="InterventionBrowseLeafName"/>
              <Field Name="InterventionBrowseLeafAsFound"/>
              <Field Name="InterventionBrowseLeafRelevance"/>
            </Struct>
          </List>
          <List Name="InterventionBrowseBranchList">
            <Struct Name="InterventionBrowseBranch">
              <Field Name="InterventionBrowseBranchAbbrev"/>
              <Field Name="InterventionBrowseBranchName"/>
            </Struct>
          </List>
        </Struct>
      </Struct>
    </Struct>
  </ElmtDefs>
</StudyStructure>
```