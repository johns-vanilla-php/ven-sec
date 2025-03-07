-- phpMyAdmin SQL Dump
-- version 5.2.1
-- https://www.phpmyadmin.net/
--
-- Host: localhost:3306
-- Generation Time: Mar 03, 2025 at 06:48 PM
-- Server version: 10.6.21-MariaDB-cll-lve
-- PHP Version: 8.3.15

SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO";
START TRANSACTION;
SET time_zone = "-05:00";


/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8mb4 */;

--
-- Database: `vs_db`
--

-- --------------------------------------------------------

--
-- Constraints for dumped tables
--

--
-- Constraints for table `attendance`
--
ALTER TABLE `attendance`
  ADD CONSTRAINT `attendance_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `attendance_ibfk_2` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE CASCADE;

--
-- Constraints for table `audit_logs`
--
ALTER TABLE `audit_logs`
  ADD CONSTRAINT `audit_logs_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE;

--
-- Constraints for table `contracts`
--
ALTER TABLE `contracts`
  ADD CONSTRAINT `contracts_ibfk_1` FOREIGN KEY (`company_id`) REFERENCES `companies` (`company_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `contracts_ibfk_2` FOREIGN KEY (`client_id`) REFERENCES `clients` (`client_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `contracts_ibfk_3` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE CASCADE;

--
-- Constraints for table `contract_amendments`
--
ALTER TABLE `contract_amendments`
  ADD CONSTRAINT `contract_amendments_ibfk_1` FOREIGN KEY (`contract_id`) REFERENCES `contracts` (`contract_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `contract_amendments_ibfk_2` FOREIGN KEY (`amended_by`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `contract_amendments_ibfk_3` FOREIGN KEY (`reviewed_by`) REFERENCES `users` (`user_id`) ON DELETE CASCADE;

--
-- Constraints for table `current_payrates`
--
ALTER TABLE `current_payrates`
  ADD CONSTRAINT `current_payrates_ibfk_1` FOREIGN KEY (`payscale_id`) REFERENCES `payscales` (`payscale_id`) ON DELETE CASCADE;

--
-- Constraints for table `daily_payroll`
--
ALTER TABLE `daily_payroll`
  ADD CONSTRAINT `daily_payroll_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `daily_payroll_ibfk_2` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `daily_payroll_ibfk_3` FOREIGN KEY (`payscale_id`) REFERENCES `payscales` (`payscale_id`) ON DELETE CASCADE;

--
-- Constraints for table `emergency_contacts`
--
ALTER TABLE `emergency_contacts`
  ADD CONSTRAINT `emergency_contacts_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE;

--
-- Constraints for table `employee_performance_reviews`
--
ALTER TABLE `employee_performance_reviews`
  ADD CONSTRAINT `employee_performance_reviews_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `employee_performance_reviews_ibfk_2` FOREIGN KEY (`reviewer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE;

--
-- Constraints for table `employment_history`
--
ALTER TABLE `employment_history`
  ADD CONSTRAINT `employment_history_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `employment_history_ibfk_2` FOREIGN KEY (`company_id`) REFERENCES `companies` (`company_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `employment_history_ibfk_3` FOREIGN KEY (`client_id`) REFERENCES `clients` (`client_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `employment_history_ibfk_4` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE CASCADE;

--
-- Constraints for table `incident_reports`
--
ALTER TABLE `incident_reports`
  ADD CONSTRAINT `incident_reports_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `incident_reports_ibfk_2` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `incident_reports_ibfk_3` FOREIGN KEY (`shift_id`) REFERENCES `shift_assignments` (`assignment_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `incident_reports_ibfk_4` FOREIGN KEY (`report_by`) REFERENCES `users` (`user_id`) ON DELETE CASCADE;

--
-- Constraints for table `incident_report_actions`
--
ALTER TABLE `incident_report_actions`
  ADD CONSTRAINT `incident_report_actions_ibfk_1` FOREIGN KEY (`incident_id`) REFERENCES `security_breaches` (`breach_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `incident_report_actions_ibfk_2` FOREIGN KEY (`action_taken_by`) REFERENCES `users` (`user_id`) ON DELETE CASCADE;

--
-- Constraints for table `incident_responses`
--
ALTER TABLE `incident_responses`
  ADD CONSTRAINT `incident_responses_ibfk_1` FOREIGN KEY (`breach_id`) REFERENCES `security_breaches` (`breach_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `incident_responses_ibfk_2` FOREIGN KEY (`created_by`) REFERENCES `users` (`user_id`) ON DELETE CASCADE;

--
-- Constraints for table `leave_history`
--
ALTER TABLE `leave_history`
  ADD CONSTRAINT `leave_history_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `leave_history_ibfk_2` FOREIGN KEY (`supervisor_id`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `log_entries`
--
ALTER TABLE `log_entries`
  ADD CONSTRAINT `log_entries_ibfk_1` FOREIGN KEY (`log_id`) REFERENCES `shift_logs` (`log_id`) ON DELETE CASCADE;

--
-- Constraints for table `log_entry_media`
--
ALTER TABLE `log_entry_media`
  ADD CONSTRAINT `log_entry_media_ibfk_1` FOREIGN KEY (`log_entry_id`) REFERENCES `log_entries` (`entry_id`) ON DELETE CASCADE;

--
-- Constraints for table `notifications`
--
ALTER TABLE `notifications`
  ADD CONSTRAINT `notifications_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `notifications_ibfk_2` FOREIGN KEY (`notification_type_id`) REFERENCES `notification_types` (`notification_type_id`) ON DELETE CASCADE;

--
-- Constraints for table `overtime_requests`
--
ALTER TABLE `overtime_requests`
  ADD CONSTRAINT `overtime_requests_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `overtime_requests_ibfk_2` FOREIGN KEY (`supervisor_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `overtime_requests_ibfk_3` FOREIGN KEY (`shift_id`) REFERENCES `shift_assignments` (`assignment_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `overtime_requests_ibfk_4` FOREIGN KEY (`approved_by`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `scheduled_time_off`
--
ALTER TABLE `scheduled_time_off`
  ADD CONSTRAINT `scheduled_time_off_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `scheduled_time_off_ibfk_2` FOREIGN KEY (`supervisor_id`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `security_breaches`
--
ALTER TABLE `security_breaches`
  ADD CONSTRAINT `security_breaches_ibfk_1` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `security_breaches_ibfk_2` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `security_breaches_ibfk_3` FOREIGN KEY (`reported_by`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `security_breaches_ibfk_4` FOREIGN KEY (`resolved_by`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `shift_assignments`
--
ALTER TABLE `shift_assignments`
  ADD CONSTRAINT `shift_assignments_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_assignments_ibfk_2` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_assignments_ibfk_3` FOREIGN KEY (`shift_supervisor`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `shift_changes`
--
ALTER TABLE `shift_changes`
  ADD CONSTRAINT `shift_changes_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_changes_ibfk_2` FOREIGN KEY (`original_shift_id`) REFERENCES `shift_assignments` (`assignment_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_changes_ibfk_3` FOREIGN KEY (`new_shift_id`) REFERENCES `shift_assignments` (`assignment_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_changes_ibfk_4` FOREIGN KEY (`approved_by`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `shift_logs`
--
ALTER TABLE `shift_logs`
  ADD CONSTRAINT `shift_logs_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_logs_ibfk_2` FOREIGN KEY (`shift_id`) REFERENCES `shift_assignments` (`assignment_id`) ON DELETE CASCADE;

--
-- Constraints for table `shift_reports`
--
ALTER TABLE `shift_reports`
  ADD CONSTRAINT `shift_reports_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_reports_ibfk_2` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_reports_ibfk_3` FOREIGN KEY (`regional_supervisor_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_reports_ibfk_4` FOREIGN KEY (`site_supervisor_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_reports_ibfk_5` FOREIGN KEY (`shift_supervisor_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_reports_ibfk_6` FOREIGN KEY (`liaison_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE;

--
-- Constraints for table `shift_swaps`
--
ALTER TABLE `shift_swaps`
  ADD CONSTRAINT `shift_swaps_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_swaps_ibfk_2` FOREIGN KEY (`original_shift_id`) REFERENCES `shift_assignments` (`assignment_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_swaps_ibfk_3` FOREIGN KEY (`swap_with_officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `shift_swaps_ibfk_4` FOREIGN KEY (`approved_by`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `task_assignments`
--
ALTER TABLE `task_assignments`
  ADD CONSTRAINT `task_assignments_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `task_assignments_ibfk_2` FOREIGN KEY (`assigned_by`) REFERENCES `users` (`user_id`) ON DELETE CASCADE;

--
-- Constraints for table `time_off_requests`
--
ALTER TABLE `time_off_requests`
  ADD CONSTRAINT `time_off_requests_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `time_off_requests_ibfk_2` FOREIGN KEY (`approved_by`) REFERENCES `users` (`user_id`) ON DELETE SET NULL,
  ADD CONSTRAINT `time_off_requests_ibfk_3` FOREIGN KEY (`covered_by`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `training_sessions`
--
ALTER TABLE `training_sessions`
  ADD CONSTRAINT `training_sessions_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `training_sessions_ibfk_2` FOREIGN KEY (`trainer_id`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `users`
--
ALTER TABLE `users`
  ADD CONSTRAINT `users_ibfk_1` FOREIGN KEY (`current_company`) REFERENCES `companies` (`company_id`) ON DELETE SET NULL,
  ADD CONSTRAINT `users_ibfk_2` FOREIGN KEY (`current_client`) REFERENCES `clients` (`client_id`) ON DELETE SET NULL,
  ADD CONSTRAINT `users_ibfk_3` FOREIGN KEY (`current_worksite`) REFERENCES `worksites` (`worksite_id`) ON DELETE SET NULL;

--
-- Constraints for table `vehicle_accident_history`
--
ALTER TABLE `vehicle_accident_history`
  ADD CONSTRAINT `vehicle_accident_history_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_assignments`
--
ALTER TABLE `vehicle_assignments`
  ADD CONSTRAINT `vehicle_assignments_ibfk_1` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `vehicle_assignments_ibfk_2` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_damage_claims`
--
ALTER TABLE `vehicle_damage_claims`
  ADD CONSTRAINT `vehicle_damage_claims_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `vehicle_damage_claims_ibfk_2` FOREIGN KEY (`incident_report_id`) REFERENCES `security_breaches` (`breach_id`) ON DELETE SET NULL;

--
-- Constraints for table `vehicle_damage_history`
--
ALTER TABLE `vehicle_damage_history`
  ADD CONSTRAINT `vehicle_damage_history_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_fleet_management`
--
ALTER TABLE `vehicle_fleet_management`
  ADD CONSTRAINT `vehicle_fleet_management_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `vehicle_fleet_management_ibfk_2` FOREIGN KEY (`assigned_to`) REFERENCES `users` (`user_id`) ON DELETE SET NULL,
  ADD CONSTRAINT `vehicle_fleet_management_ibfk_3` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE SET NULL;

--
-- Constraints for table `vehicle_fuel_purchase`
--
ALTER TABLE `vehicle_fuel_purchase`
  ADD CONSTRAINT `vehicle_fuel_purchase_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_incident_response`
--
ALTER TABLE `vehicle_incident_response`
  ADD CONSTRAINT `vehicle_incident_response_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `vehicle_incident_response_ibfk_2` FOREIGN KEY (`incident_id`) REFERENCES `security_breaches` (`breach_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_inspections`
--
ALTER TABLE `vehicle_inspections`
  ADD CONSTRAINT `vehicle_inspections_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_inspections_history`
--
ALTER TABLE `vehicle_inspections_history`
  ADD CONSTRAINT `vehicle_inspections_history_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_inspection_history_log`
--
ALTER TABLE `vehicle_inspection_history_log`
  ADD CONSTRAINT `vehicle_inspection_history_log_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `vehicle_inspection_history_log_ibfk_2` FOREIGN KEY (`inspector_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_inspection_schedule`
--
ALTER TABLE `vehicle_inspection_schedule`
  ADD CONSTRAINT `vehicle_inspection_schedule_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `vehicle_inspection_schedule_ibfk_2` FOREIGN KEY (`inspector_id`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `vehicle_insurance`
--
ALTER TABLE `vehicle_insurance`
  ADD CONSTRAINT `vehicle_insurance_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_insurance_claims`
--
ALTER TABLE `vehicle_insurance_claims`
  ADD CONSTRAINT `vehicle_insurance_claims_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `vehicle_insurance_claims_ibfk_2` FOREIGN KEY (`incident_report_id`) REFERENCES `security_breaches` (`breach_id`) ON DELETE SET NULL;

--
-- Constraints for table `vehicle_insurance_expiry`
--
ALTER TABLE `vehicle_insurance_expiry`
  ADD CONSTRAINT `vehicle_insurance_expiry_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_lease_agreements`
--
ALTER TABLE `vehicle_lease_agreements`
  ADD CONSTRAINT `vehicle_lease_agreements_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_licensing`
--
ALTER TABLE `vehicle_licensing`
  ADD CONSTRAINT `vehicle_licensing_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_maintenance_logs`
--
ALTER TABLE `vehicle_maintenance_logs`
  ADD CONSTRAINT `vehicle_maintenance_logs_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_mileage_history`
--
ALTER TABLE `vehicle_mileage_history`
  ADD CONSTRAINT `vehicle_mileage_history_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_performance_monitoring`
--
ALTER TABLE `vehicle_performance_monitoring`
  ADD CONSTRAINT `vehicle_performance_monitoring_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_registration`
--
ALTER TABLE `vehicle_registration`
  ADD CONSTRAINT `vehicle_registration_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_registration_expiry`
--
ALTER TABLE `vehicle_registration_expiry`
  ADD CONSTRAINT `vehicle_registration_expiry_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_registration_renewal`
--
ALTER TABLE `vehicle_registration_renewal`
  ADD CONSTRAINT `vehicle_registration_renewal_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_service_history`
--
ALTER TABLE `vehicle_service_history`
  ADD CONSTRAINT `vehicle_service_history_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_service_provider_services`
--
ALTER TABLE `vehicle_service_provider_services`
  ADD CONSTRAINT `vehicle_service_provider_services_ibfk_1` FOREIGN KEY (`provider_id`) REFERENCES `vehicle_service_provider` (`provider_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `vehicle_service_provider_services_ibfk_2` FOREIGN KEY (`service_id`) REFERENCES `vehicle_services` (`service_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_service_reminder`
--
ALTER TABLE `vehicle_service_reminder`
  ADD CONSTRAINT `vehicle_service_reminder_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_service_schedule`
--
ALTER TABLE `vehicle_service_schedule`
  ADD CONSTRAINT `vehicle_service_schedule_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_taxation`
--
ALTER TABLE `vehicle_taxation`
  ADD CONSTRAINT `vehicle_taxation_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_tire_history`
--
ALTER TABLE `vehicle_tire_history`
  ADD CONSTRAINT `vehicle_tire_history_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `vehicle_warranty`
--
ALTER TABLE `vehicle_warranty`
  ADD CONSTRAINT `vehicle_warranty_ibfk_1` FOREIGN KEY (`vehicle_id`) REFERENCES `vehicles` (`vehicle_id`) ON DELETE CASCADE;

--
-- Constraints for table `worksites`
--
ALTER TABLE `worksites`
  ADD CONSTRAINT `worksites_ibfk_1` FOREIGN KEY (`client_id`) REFERENCES `clients` (`client_id`) ON DELETE CASCADE;

--
-- Constraints for table `worksite_policies`
--
ALTER TABLE `worksite_policies`
  ADD CONSTRAINT `worksite_policies_ibfk_1` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `worksite_policies_ibfk_2` FOREIGN KEY (`created_by`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `worksite_policies_ibfk_3` FOREIGN KEY (`approved_by`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;

--
-- Constraints for table `worksite_schedules`
--
ALTER TABLE `worksite_schedules`
  ADD CONSTRAINT `worksite_schedules_ibfk_1` FOREIGN KEY (`worksite_id`) REFERENCES `worksites` (`worksite_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `worksite_schedules_ibfk_2` FOREIGN KEY (`officer_id`) REFERENCES `users` (`user_id`) ON DELETE CASCADE,
  ADD CONSTRAINT `worksite_schedules_ibfk_3` FOREIGN KEY (`payscale_id`) REFERENCES `payscales` (`payscale_id`) ON DELETE SET NULL,
  ADD CONSTRAINT `worksite_schedules_ibfk_4` FOREIGN KEY (`shift_supervisor_id`) REFERENCES `users` (`user_id`) ON DELETE SET NULL;
COMMIT;

/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
